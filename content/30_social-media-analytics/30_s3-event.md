+++
title = "Lambda Function and S3 Event Notification"
chapter = false
weight = 30
+++

After the CloudFormation stack launch is completed, go to the outputs tab for direct links and information. Then click the **LambdaFunctionConsoleURL** link to launch directly into the Lambda function.

The Lambda function calls Amazon Translate and Amazon Comprehend to perform language translation and natural language processing (NLP) on tweets. The function uses Amazon Kinesis to write the analyzed data to Amazon S3.

![architecture](/images/social-media-analytics/twitter-dashboard-3.gif)

Most of this has been set up already by the CloudFormation stack, although we will have you add the S3 notification so that the Lambda function is invoked when new tweets are written to S3:

1. Under **Add Triggers**, select the S3 trigger.
2. Then configure the trigger with the new S3 bucket that CloudFormation created with the `raw/` prefix. The event type should be `Object Created (All)`.

    Following least privilege patterns, the IAM role that the Lambda function has been assigned only has access to the S3 bucket that the CloudFormation template created.

The following diagram shows an example:

![s3-event-console](/images/social-media-analytics/twitter-dashboard-8.gif)

Take some time to examine the rest of the code below. With a few lines of code, we can call Amazon Translate to convert between Arabic, Portuguese, Spanish, French, German, English, and many other languages.

The same is true for adding natural language processing into the application using Amazon Comprehend. Note how easily we were able to perform the sentiment analysis and entity extraction on the tweets within the Lambda function.

```javascript
'use strict';
const aws = require('aws-sdk');
const async = require('async');

const s3 = new aws.S3({ apiVersion: '2006-03-01' });
const comprehend = new aws.Comprehend();
const translate = new aws.Translate();
const firehose = new aws.Firehose();

var sentimentStream = process.env.SENTIMENT_STREAM;
var entityStream = process.env.ENTITY_STREAM;

exports.handler = (event, context, lambdaCallback) => {

    const bucket = event.Records[0].s3.bucket.name;
    const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' '));
    const params = {
        Bucket: bucket,
        Key: key,
    };
    s3.getObject(params, (err, data) => {
        if (err) {
            console.log(err);
            lambdaCallback(err);
        } else {
            var tweets = data.Body.toString('utf8');

            //lets go through each line in the doc now.
            var tweetArray = tweets.split('\n');

            var count = 0;

            async.forEachSeries(
                tweetArray, 
                function(tweetLine, itemCallback)
                {
                  if(tweetLine.length > 0)
                    {
                      count++;

                      var tweet = JSON.parse(tweetLine);
                      async.waterfall([
                        //*********************************
                        //
                        //  Translate if needed function call.
                        //
                        //*********************************
                        function(doneTranslatingCallback){
                          if(tweet.lang !== 'en')
                          {
                            var translateParams = {
                                SourceLanguageCode: tweet.lang,
                                TargetLanguageCode: 'en', 
                                Text: tweet.text 
                              };
                              translate.translateText(translateParams, function(err, data) {
                                if (err){
                                  console.log('Error Translating: ' + err, err.stack);
                                  doneTranslatingCallback(err);
                                }
                                else{
                                  console.log('Translated tweet\n\ttranslated: ' + data.TranslatedText + '\n\tfrom: ' + tweet.text);
                                  //This is a async callback...so need to do the call here or us a wait method..
                                  //will call here since it's not a lot of callback chaingin.
                                  tweet.originalText = tweet.text;
                                  tweet.text = data.TranslatedText;
                                  doneTranslatingCallback(null);
                                }

                              });
                          }//end of if not english
                          else
                          {
                            doneTranslatingCallback(null);
                          }
                        },
                        //*********************************
                        //
                        //  Run Sentiment Analysis
                        //
                        //*********************************
                        function(sentimentCallback){
                            var params = {
                                  LanguageCode: 'en', /* required */
                                  Text: tweet.text
                                };

                            comprehend.detectSentiment(params, function(err, data) {
                              if (err){
                                console.log(err, err.stack); // an error occurred
                                sentimentCallback(err);
                              }
                              else{
                                  var sentimentRecord = {
                                          DeliveryStreamName: sentimentStream, 
                                          Record: { 
                                                Data: JSON.stringify({
                                                      tweetid: tweet.id, /* required */
                                                      text: tweet.text,
                                                      originalText: tweet.originalText,
                                                      sentiment: data.Sentiment,
                                                      sentimentPosScore: Number((data.SentimentScore.Positive).toFixed(3)),
                                                      sentimentNegScore: Number((data.SentimentScore.Negative).toFixed(3)),
                                                      sentimentNeuScore: Number((data.SentimentScore.Neutral).toFixed(3)),
                                                      sentimentMixedScore: Number((data.SentimentScore.Mixed).toFixed(3))
                                                    }) + '\n'
                                            }//end of record
                                        };//end of params
                                        firehose.putRecord(sentimentRecord, function(err, data) {
                                          if (err)
                                          { 
                                            console.log(err, err.stack);
                                            sentimentCallback(err);
                                          }
                                          else
                                            sentimentCallback(null);
                                        });
                                }    
                            });
                        },
                        //*********************************
                        //
                        //  Run Entity Extraction
                        //
                        //*********************************
                        function(entityCallback){
                           var params = {
                                  LanguageCode: 'en', /* required */
                                  Text: tweet.text
                                };

                          comprehend.detectEntities(params, function(err, data) {
                              if (err){
                                console.log(err, err.stack); // an error occurred
                                entityCallback(err);
                              }
                              else{
                                  data.Entities.forEach(function(entity){
                                       var entityRecord = {
                                          DeliveryStreamName: entityStream, 
                                          Record: { 
                                                Data: JSON.stringify({
                                                      tweetid: tweet.id, /* required */
                                                      entity: entity.Text,
                                                      type: entity.Type,
                                                      score: entity.Score
                                                    }) + '\n'
                                            }//end of record
                                        };//end of params
                                        firehose.putRecord(entityRecord, function(err, data) {
                                          if (err) console.log(err, err.stack);
                                        });
                                  });
                                  entityCallback(null);
                                }
                            });
                        }
                    ],
                    //******************************************************
                    //
                    // Callback handler for waterfall flow -- this is when we call back to the item forEach...
                    //
                    //******************************************************
                    function (err, result) {
                        if(err)
                        {
                          console.log('exception processing "' + tweetLine + '" ' + err);
                        }
                        itemCallback();
                    });
                  }//is the tweet > 0
                }, 
                //******************************************************
                //
                // Callback handler for forEach block -- this is when we want to finish the lambda exec...
                //
                //******************************************************
                function(error)
                {
                  console.log('processed ' + count + ' tweets');
                  lambdaCallback(null, true);
                });//end async for each...
        }
    });
};
```