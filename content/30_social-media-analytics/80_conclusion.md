+++
title = "Conclusion"
chapter = false
weight = 80
+++

The entire processing, analytics, and machine learning pipeline starting with Amazon Kinesis, analyzing the data using Amazon Translate to translate tweets between languages, using Amazon Comprehend to perform sentiment analysis and QuickSight to create the dashboards was built without spinning up any servers.

We added advanced machine learning (ML) services to our flow, through some simple calls within AWS Lambda, and we built a multi-lingual analytics dashboard with Amazon QuickSight. We have also saved all the data to Amazon S3 so, if we want, we can do other analytics on the data using Amazon EMR, Amazon SageMaker, Amazon Elasticsearch Service, or other AWS services.

Instead of running the Amazon EC2 instance that reads the Twitter firehose, you could leverage AWS Fargate to deploy that code as a container. AWS Fargate is a technology for Amazon Elastic Container Service (ECS) and Amazon Elastic Container Service for Kubernetes (EKS) that allows you to run containers without having to manage servers or clusters. With AWS Fargate, you no longer have to provision, configure, and scale clusters of virtual machines to run containers. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing. AWS Fargate removes the need for you to interact with or think about servers or clusters. Using AWS Fargate you can focus on designing and building your applications instead of managing the infrastructure that runs them.