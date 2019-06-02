+++
title = "Shutting Down"
chapter = false
weight = 70
+++

After you have created these resources, you can remove them by following these steps.

1. Stop the Twitter stream reader (if you still have it running).

    * (While SSH'd into the server) CTRL-C or kill it if it’s in the background.

2. Delete the S3 bucket that the CloudFormation template created.
3. Delete the Athena tables database (`socialanalyticsblog`).

    * Drop table socialanalyticsblog.tweets.
    * Drop table socialanalyticsblog.tweet_entities.
    * Drop table socialanayticsblog.tweet_sentiments.
    * Drop database socialanalyticsblog.

4. Delete the CloudFormation stack (ensure that the S3 bucket is empty prior to deleting the stack).
