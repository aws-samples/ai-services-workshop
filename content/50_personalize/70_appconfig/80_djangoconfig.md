+++
title = "Django Framework Config"
chapter = false
weight = 80
+++

There are various components within the application that need some final configuration.  The basics, such as the VPC, the Application Load Balancer, the Auto-Scaling Group and resultant EC2 images, are all good to go, but some configuration is necessary on the Django application framework that is hosting the application.  This needs to be done by connecting into the instance using SSH.

There are many ways to connect to a remote machine using SSH. Feel free to follow the instructions from the [prerequisites]({{<ref "20_installs.md#create-an-ec2-keypair">}}). This Lab Guide will continue with using SSH at the command line on an Apple Mac computer - your own method for establishing a connection may be different, but once connected the instructions are the same regardless of your platform combination.

1. In order to connect you need to have your downloaded key-pair from earlier in an accessible location.  It also must not be publicly readable, so if you are on a Mac or Linux system you can fix this with the following command, remembering to replace **myLabKey.pem** with your key name!

      ```bash
      $ chmod 400 myLabKey.pem
      ```

2. Go to the the EC2 console page, go to the **Instances** menu on the left, and find your one running instance.  Select it, and make a note of (or copy) the **IPv4 Public IP** for your instance

      ![ec2 ip image](/images/selectEC2details.png)

3. Go to your computer CLI, and navigate to the directory containing your key-pair.  Issue the following command to connect via SSH, changing the key-pair filename and IP address as necessary, and you should see results similar to what follows.  You will see a warning about the authenticity of the host then just enter **yes** at the prompt.

      ```bash
      $ ssh -i myLabKey.pem ec2-user@3.87.13.157

      The authenticity of host '3.87.13.157 (3.87.13.157)' can't be established.
      ECDSA key fingerprint is SHA256:hFLzWhKWXwSevk14ulMwyLJqM7LN7j3Yt5w7NcnNwow.
      Are you sure you want to continue connecting (yes/no)? yes
      Warning: Permanently added '3.87.13.157' (ECDSA) to the list of known hosts.

      Last login: Sun May  5 20:51:33 2019 from 72-21-198-64.amazon.com

            __|  __|_  )
            _|  (     /   Amazon Linux 2 AMI
            ___|\___|___|

      https://aws.amazon.com/amazon-linux-2/
      [ec2-user@ip-10-192-11-223 ~]$
      ```

4. Navigate to the root of the Django project, and configure the single-line run script to use the private IP address of the EC2 instance.  This is on the previous EC2 details screen just underneath the Public IP - you will need this again, so keep it handy.  Simply replace the IP address with yours, keeping the trailing **:8000** - my server's private IP is 10.192.11.223.

      ```bash
      $ cd personalize-video-recs/videorecs/
      $ nano runmyserver
      --- {editor screen} ---
      python manage.py runserver 10.192.11.223:8000
      ```

5. Django only allows access via pre-defined source IP addresses.  Naturally, these could be open to the internet, but they recommend only exposing it the instance private IP address (for internal calls) and to your front-end load balancer.  You already have a reference to the private IP address, so you now need to extract the Load Balancer DNS entry.  Go back to the EC2 console screen, but this time select **Load Balancers** on the left-hand menu; select your Application Load Balancer and in the details screen that comes up select the DNS name and store it for later.

      ![load balancer image](/images/loadbalancerDNS.png)

6. Whilst we're collecting data, move to the **Amazon RDS** service section of the console, select **Dartabases** from the left-hand menu and select the Lab database called **summitpersonalizelab** from the list.  In the details screen copy the DNS endpoint for the database and store it for later.

      ![dns record image](/images/rdsDNS.png)

7. Go back to your SSH session window.  You now need to edit two entries in the file - one is called **ALLOWED_HOSTS** and the other is **HOST** entry in the **DATABASES** section.  Edit the file, then in the editor window find the two relevant lines and edit them so that they look like that shown below, but with your IP and DNS entries

      ```bash
      $ nano videorecs/settings.py
      --- {ALLOWED_HOSTS line - server private IP and ALB DNS} ---

      ALLOWED_HOSTS = ['10.192.11.151', 'TestS-Appli-ADS60FMCKPMG-1862985075.us-east-1.elb.amazonaws.com']

      --- {DATABASES HOSTS line - RDS DNS} ---

            'HOST': 'summitpersonalizelab.c0azewoaia5d.us-east-1.rds.amazonaws.com',
      ```

8. Finally, the RDS database is postgres, and we have included the **pgcli** tool with this deployment.  If you wish to use it then you need to edit the startup script for the utility to point to the RDS DNS entry.  You also need to know the password, which you may have noticed in the **settings.py** file, and it's **recPassw0rd**

      ```bash
      $ nano pgcli
      --- {editor screen} ---
      /home/ec2-user/.local/bin/pgcli -h summitpersonalizelab.c0azewoaia5d.us-east-1.rds.amazonaws.com -u vidrecdemo -d videorec
      ```

9. You are now ready to run the application server!  Simply execute the **runmyserver** script, and you should see status messages appearing quickly - these initial ones are the Load Balancer health-checks, and after a minute or so the instance should be declared healthy by the Load Balancer Target Group.  Note, you will see some warnings around the *psycopg2* component, but this can be ignored.

      ```bash
      $ ./runmyserver

      System check identified no issues (0 silenced).
      May 06, 2019 - 14:53:03
      Django version 1.11.18, using settings 'videorecs.settings'
      Starting development server at http://10.192.11.223:8000/
      Quit the server with CONTROL-C.
      [06/May/2019 14:53:14] "GET /recommend/ HTTP/1.1" 200 2893
      [06/May/2019 14:53:32] "GET /recommend/ HTTP/1.1" 200 2893
      [06/May/2019 14:53:44] "GET /recommend/ HTTP/1.1" 200 2893
      ```

10. The URL of the server is your ALB followed by the '/recommend/' path, although there is also an '/admin/' path that we'll use later.  For now connect to your server - in my example the server can be found at http://TestS-Appli-ADS60FMCKPMG-1862985075.us-east-1.elb.amazonaws.com/recommend

11. You should see the following screen in your browser - no *Model Precision Metrics* are available, as we haven't added any models yet to the application.  You can also see that documentation for this is present, but be aware that it may not be 100% up to date with coding changes on the demo.

      ![app screen](/images/appFrontScreen.png)

12. If you hit **Select Random User** then you'll be taken to the main Recommendation screen, which starts by showing you a random user's top-25 movie review titles.  However, you'll see on the Model dropdown on the left that there are no models available, and if you change the Personalize Mode to either Personal Ranking or Similar Items then it's the same story - you can see the movie reviews, and most-popular titles in a genre, but no recommendations.  We need to get the solutions and campaigns built in the notebook, then you can come back and plug in the models.

      ![app screen](/images/appRecNoModels.png)

13. At this point your application server doesn't actually have any credentials to call the APIs - up until this point we haven't had to call any, but we soon will.  At the beginning of this Lab you were asked to copy a block of credentials from the **Console Login** screen - please retrieve these, and in your SSH session window hit CTRL-C to stop the web server, and paste them in, which will look something like the following.  Don't forget to press [RETURN] after the **AWS_DEFAULT_REGION** line in case it didn't copy

      ```bash
      $ export AWS_ACCESS_KEY_ID=******
      $ export AWS_SECRET_ACCESS_KEY=******
      $ export AWS_SESSION_TOKEN=************************************
      $ export AWS_DEFAULT_REGION=us-east-1
      ```

At this point we require the solution that is being built in the notebook to complete - until that time we cannot move forward, so you may wish to get some refreshments if you are still waiting for that to complete.