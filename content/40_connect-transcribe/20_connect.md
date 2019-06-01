+++
title = "Create and Configure an Amazon Connect Instance"
chapter = false
weight = 20
+++

## Provisioning an Amazon Connect Instance

To get started we will provision an Amazon Connect instance.

1. First navigate to the [AWS Connect Console](https://console.aws.amazon.com/connect/).
2. Now click the **Add an instance** button
    ![AWS Connect Add Instance Button](/images/connect-transcribe/1-connect-add-instance.png)
3. To use Amazon Connect we'll need a user directory which we can create in the add instance wizard. Use a unique name for the URL like `ai-services-workshop-<myname>`.
    ![AWS Connect Identity Management](/images/connect-transcribe/2-connect-identity-management.png)
4. Create an administrator user and click **Next Step**.
    ![image showing creating an admin user](/images/connect-transcribe/3-administrator.png)
5. You can click through to the next screen accepting the default telephony options.
    ![telephony configuration showing both boxes checked for inbound and outbound](/images/connect-transcribe/4-telephony.png)
6. You can also accept the default options for data storage and click **Next step**.
    ![data storage options in amazon connect](/images/connect-transcribe/5-data-storage.png)
7. Review the options and click **Create Instance**
    ![review options and create instance](/images/connect-transcribe/6-review-create.png)
8. From there we can login to the connect instance with out administrator user.

## Configuring Connect Instance

The first thing we can do is go through the Connect first launch wizard as we login. This will prompt us to select a phone number and supply a few sensible defaults.
![connect welcome](/images/connect-transcribe/connect-instance/1-welcome.png)
![connect phone number selection](/images/connect-transcribe/connect-instance/2-claim-phone-number.png)

{{% notice note %}}
It may take a few minutes for the number to become active.
{{% /notice %}}

The rest of the configuration takes place back in the [AWS Console for Connect](https://console.aws.amazon.com/connect/).

1. First click the link under **Instance Alias** that corresponds to the instance you just created.
2. Take note of the **Instance ARN** on this page for later, the value at the end of the ARN is what we will use to provision our CloudFormation resources later.
    ![highlighted instance arn](/images/connect-transcribe/connect-instance/connect-instance-arn.png)
3. Now click on the **Data storage** option on the far left and click **Edit** under the **Live media streaming** heading.
    ![data storage console in amazon connect](/images/connect-transcribe/connect-instance/connect-data-storage.png)
4. From here create a prefix for your Amazon Kinesis Video (KVS) streams and select an encryption key and data retention period. Then click **Save** in the top right corner and click **Save** again in the bottom right corner.
    ![live media streaming configuration](/images/connect-transcribe/connect-instance/connect-live-media-streaming.png)

Remember to take note of the instance alias and instance ARN - we'll use those in the next step.
