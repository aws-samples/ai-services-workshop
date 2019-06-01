+++
title = "Using the Agent Interface"
chapter = false
weight = 50
+++

## Finding The Agent Interface

The agent interface is one of the outputs of the CloudFormation script we used to create our resources. We can find it easily in the **Outputs** tab of the stack in the CloudFormation console. Navigate there and copy the URL.

![agent url in cloudformation console](/images/connect-transcribe/cloudformation-agent-url.png)

## Adding Agent Interface As A Trusted Origin

Now we'll go back to the AWS Console for Connect and edit our instance again. We want to go to the **Application Integration** options where we'll add the CloudFront URL as a trusted origin.

![trusted origin](/images/connect-transcribe/cloudformation-agent-url.png)

## Launching the Agent Interface

{{% notice note %}}
Try running this in incognito mode if you have any issues.
{{% /notice %}}

From there we simply navigate to the CloudFront URL and login to Connect. That will start the agent interface and once we set ourselves to available we can begin receiving calls. 

When we call our number, if everything is working, we should get output that is streaming from the call in real time. If it has been a long time since you ran the CloudFormation script and you face issues with the calls or transcription try waiting 30 seconds for the transcription to start. Subsequent calls will start transcription instantly.
![agent console](/images/connect-transcribe/agent-console.png)

You can learn more about this solution by reading the [guide](https://s3.amazonaws.com/solutions-reference/AI-powered-speech-analytics-for-amazon-connect/latest/AI+Powered+Speech+Analytics+for+Amazon+Connect.pdf).
