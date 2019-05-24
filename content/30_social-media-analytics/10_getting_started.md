+++
title = "Getting Started"
chapter = false
weight = 10
+++

### Build a social media dashboard using machine learning and BI services

In this workshop we'll make use of:

* [Amazon Translate](https://aws.amazon.com/translate/)
* [Amazon Comprehend](https://aws.amazon.com/comprehend/)
* [Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/)
* [Amazon Athena](https://aws.amazon.com/athena/)
* [Amazon Quicksight](https://aws.amazon.com/quicksight/)
* [Amazon S3](https://aws.amazon.com/s3/)

We'll use these tools to build a natural-language-processing (NLP)-powered social media dashboard for tweets.

Social media interactions between organizations and customers deepend brand awareness and these conversations are a low-cost way to acquire leads, improve traffic, develop relationships, measure brand sentiment, and improve customer service.

A major shoutout to Ben Snively and Viral Desai for their original work on this. You can read their [blog post](https://aws.amazon.com/blogs/machine-learning/build-a-social-media-dashboard-using-machine-learning-and-bi-services/) for more.


### What It Looks Like

This is what our final dashboard will look like:

![dashboard of social media data](/images/social-media-analytics/twitter-dashboard-1.gif)



### CloudFormation

We'll provsion and deploy this workshop with a tool called [AWS CloudFormation](https://aws.amazon.com/cloudformation/) which allows us to define our infrastructure as code. In this case, YAML.

We can proceed with provisioning the workshop resources by using the "Launch Stack" button below.
<p><a href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=SocialMediaAnalyticsBlogPost&amp;templateURL=https:%2F%2Fs3.amazonaws.com%2Fserverless-analytics%2FSocialMediaAnalytics-blog%2Fdeploy.yaml" target="_blank" rel="noopener noreferrer"><img class="alignnone wp-image-47 size-full" src="https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2017/02/10/launchstack.png" alt="" width="107" height="20"></a></p>


### Launching The Stack
We launch this by providing our EC2 instance keypair name and our Twitter tokens provisioned in the prequisites.

From there we can pass in a few other variables like the languages we will support and translate as well as the terms we want to search for on Twitter.

![cloudformation terms](/images/social-media-analytics/twitter-dashboard-4.gif)
