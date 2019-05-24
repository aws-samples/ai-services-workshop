+++
title = "Architecture"
chapter = false
weight = 20
+++


After we deploy the CloudFormation template we will have the following architecture:

* Amazon EC2 instance that listens to the Twitter streaming API and submits results to the Amazon Kinesis Data Firehose API
* The Kinesis Data Firehose API will stream the JSON records to Amazon S3 buckets
* The uploads of JSON records into the "/raw" location of the S3 bucket will trigger an AWS Lambda Function that takes the record and sends it to Amazon Translate and Amazon Comprehend.
* That same Lambda function will take the modified and enriched records and put them in another Kinesis Firehose that sends those records to an S3 bucket.
* We will query the JSON records with Amazon Athena
* We will visualize the output and results of this pipeline with Amazon Quicksight.

![architecture](/images/social-media-analytics/twitter-dashboard-3.gif)

