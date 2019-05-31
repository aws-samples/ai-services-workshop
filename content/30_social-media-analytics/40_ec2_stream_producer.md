+++
title = "Start the Tweet Streamer on EC2"
chapter = false
weight = 40
+++

Using the keypair we created in the initial steps we'll login to the EC2 instance and run 

```bash
node twitter_stream_producer_app.js
```

We could run this in the background with something like `nohup` if we wanted to leave it running after we disconnect.