+++
title = "Item-to-Item Recommendations"
chapter = false
weight = 50
+++

## Create Item-to-Item Similarities Solution

1. Using the same methods as before, go to the Services drop-down in the console and navigate to the **Amazon Personalize** service in another tab.  You will see the dataset group that you created earlier

    ![Dataset groups](/images/datasetGroups.png)

2. Click on the name of the your dataset group, then on the left-hand side, which will show you the solution that you're currently creating via your notebook.  Then, select **Solutions and recipes**, then click on the **Create solution** button.

   ![Solution list](/images/solutionList.png)

3. Enter a suitable name for this solution, such as *similar-items-solutions*, select **Manual** recipe selection, then choose the **aws-sims** recipe and click **Next** - we don't need to change anything in the advanced configuration section

   ![Create solution](/images/solutionConfig.png)

4. In the following screen just hit the **Finish** button and a new solution version will start to be created.