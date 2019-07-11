+++
title = "Deploy the App"
chapter = false
weight = 20
+++

## Deploy the "Video Recommendation" Algorithm

1. The application will run on an EC2 instance, but at some point we will need to connect to the server in order to carry out some configuration task. To do this we need to have an *EC2 Key Pair* configured on the server that you also have access to on your computer; hence, we need to create and download a new one.  Click on **EC2** from the list of all services by entering EC2 into the Find services box.  This will bring you to the Amazon EC2 console home page.

    ![EC2 Select](/images/consoleEC2Select.png)

2. On the left-hand menu scroll down until you see **Key Pairs** and select it, and in the resulting dialog click on the **Create Key Pair** button.  This will bring up a **Create Key Pair** dialog, where you need to enter the name of a new key pair - call it `myLabKey` and hit **Create**.  This should automatically download the file, or you may need to manually do so.

    ![Create key pair](/images/createKeyPair.png)

3. Click on the **Services** dropdown and select **CloudFormation** from the list of all services by entering CloudFormation into the Find services box.  This will bring you to the Amazon CloudFormation console home page.

    ![CFN Service Selection](/images/consoleCfnSelect.png)

4. We are going to deploy a pre-built application via a CloudFormation template - this will be a fully-functioning recommendation system, allowing access to multiple Amazon Personalize features.  But it has one drawback - there are no models built into it!  So we will create them in this lab, and when they are ready we will re-configure this application to use them. First, we'll deploy this skeleton application, which will require us to download this CloudFormation template file. Navigate to the following link, which will automatically save `cloudformation_template.yml` to your local computer.

    https://personalize-workshop-files.s3.amazonaws.com/cloudformation_template.yml

5. There will already be one stack deployed into your account, but we need to create another.  On the CloudFormation screen, click on the **Create Stack** button to start the deployment wizard, and in the **Choose a template** section select **Upload a template to Amazon S3**, click on the **Choose file** button, and select the template file that you just downloaded.  Then click on **Next**.

    ![Select CFN Template](/images/cfnSelectTemplate.png)

6. The next screen asks for more configuration parameters, but only two of these are required: **Stack name** and **KeyName**.  For Stack name enter something simple, such as *LabStack*, and select your previously-defined EC2 kay-pair, then click **Next** (not shown).

    ![other cfn params](/images/cfnOtherParams.png)

7. There will then be two additional screens. The first is called *Options*, but we have none to enter, so just click on **Next**.  The second is the final *Review* screen - **please verify** that the **KeyName** is the one that you just downloaded, and then click on **Create**.  This will then go and create the environment, which will take around 10 minutes. Once the console returns to the main CloudFormation screen, you can continue with the next lab step. 