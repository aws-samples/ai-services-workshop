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

A best practice is to deploy your infrastructure close to your customers, let's configure a default AWS region for this workshop : `Northern Virginia (*us-east-1*)` for North America or `Ireland (*eu-west-1*)` for Europe.

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


### Create an EC2 KeyPair

Creating an SSH KeyPair will allow us to connect to other EC2 instances from our cloud9 machine.

Let's run this command in our terminal:
```bash
aws ec2 create-key-pair --key-name workshop --query 'KeyMaterial' --output text > workshop.pem
```

That will create a new private key on our machine, in `workshop.pem`, and a public key for EC2 to manage. For security reasons we'll change the permissions on the `workshop.pem` file so that only we can read it.

```bash
chmod 400 workshop.pem
```

Now we can start an `ssh-agent` running the background that can manage our keys and connections for us.

```bash
eval `ssh-agent -s`
ssh-add workshop.pem
```

If it asks for a passphrase you can include one or leave it blank and just press enter.


