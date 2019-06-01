+++
title = "Provision remaining resources with CloudFormation"
chapter = false
weight = 30
+++

With our Amazon Connect instance configured we're ready to laucnh the remaining resources.

<p><a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=AIPoweredSpeechAnalytics&templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-test-reference%2FAI-powered-speech-analytics-for-amazon-connect%2Flatest%2FAI-powered-speech-analytics-for-amazon-connect.template" target="_blank" rel="noopener noreferrer"><img class="alignnone wp-image-47 size-full" src="https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2017/02/10/launchstack.png" alt="" width="107" height="20"></a></p>

Click the launch stack button above and fill out the parameters with the values we saved from the previous step.

![cloudformation console](/images/connect-transcribe/cloudformation.png)

We'll click through the next few screens and provision our S3 Bucket, KVS Streams, Lambda Functions, and other resources for this workshop. It will take about 20 minutes to provision all of these resources so this is a good time to stretch and check some emails.