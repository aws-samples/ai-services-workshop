+++
title = "Shutting Down"
chapter = false
weight = 200
+++

### Using Event Engine?

Thankfully, as part of how Event Engine accounts work, there will be no need to tear down resources, as the accounts will all be destroyed after the workshop. However, if you'd like an idea of what tearing down these resources looks like, follow the below instructions.

### Shutting Down the SageMaker Notebook Instance

1. Open the Amazon SageMaker console and click on **Notebook instances**
2. Find the notebook instance listed as _[Name]-lab-notebook_, select its radio button and then click the **Actions** dropdown.

    ![Terminate instance](/images/terminateNotebook.png)

3. Click **Stop** to stop the Notebook Instance.  This does not delete the underlying data and resources.  After a few minutes the instance status will change to _Stopped_, and you can now click on the **Actions** dropdown again, but this time select **Delete**.

Note that by selecting the name of the Notebook instance on this dialog you are taken to a more detailed information page regarding that instance, which also has **Stop** and **Delete** buttons present – notebooks can also be deleted using this method.

### Shutting Down the CloudFormation Stack

The CloudFormation stack we deployed in [this step]({{<ref "20_deploytemplate.md" >}}) contains multiple resources that we'll to terminate:

* EC2 Instance
* Application Load Balancer
* Networking: Internet Gateway, VPC, Subnets, NAT Gateways, Route Table

To delete this:

1. On the **Stacks** page in the CloudFormation console, select the stack that you want to delete. The stack must be currently running.

2. In the stack details pane, choose **Delete**.

3. Select **Delete stack** when prompted.

Note: After stack deletion has begun, you cannot abort it. The stack proceeds to the `DELETE_IN_PROGRESS` state.