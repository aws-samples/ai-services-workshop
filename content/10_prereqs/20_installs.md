+++
title = "Installs & Configs"
chapter = false
weight = 20
+++

Before we begin coding, there are a few things we need to install, update, and configure in the Cloud9 environment.

### Installing and updating

In the Cloud9 terminal, **run the following commands** to install and update some software we'll be using for this workshop:

```bash
# Update the AWS CLI
pip install --user --upgrade awscli
```

{{% notice note %}}
These commands will take a few minutes to finish.
{{% /notice %}}

### Configuring a default region 

A best practice is to deploy your infrastructure close to your customers, let's configure a default AWS region for this workshop : Northern Virginia (*us-east-1*) for North America or Ireland (*eu-west-1*) for Europe.

**Create an AWS config file**, run:

{{% tabs %}}
{{% tab "us-east-1" "North America" %}}
```bash
cat <<END > ~/.aws/config
[default]
region=us-east-1
END
```
{{% /tab %}}

{{% tab  "eu-west-1"  "Europe" %}}
```bash
cat <<END > ~/.aws/config
[default]
region=eu-west-1
END
```
{{% /tab %}}
{{% /tabs %}}

