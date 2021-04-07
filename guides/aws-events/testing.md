---
layout: home
title: AWS events testing
nav_order: 4
permalink: /guides/aws-events-testing
layout: template
description: Sending test events to Panther
parent: Panther guides
image: /img/panther_logo_thin_with_aws.png
---

# Testing everything is working

After installation has completed a new lambda function will have been registered with the supplied __Stack Name__ by default this will be **AWS-Events2Panther**. 

## Simulating an EC2 instance state change

Find your lambda functions, if you imported with default values it will be named **AWS-Events2Panther** and should be listed on [aws.amazon.com/lambda/home](https://console.aws.amazon.com/lambda/home)

You should see something like ![Default lambda function](./aws-lambda-panther.png)

### Sample test data

In order to test Events2Panther we'll need some test data, you can copy the example data below, or provide your own.

```json
{
   "id":"7bf73129-1428-4cd3-a780-95db273d1602",
   "detail-type":"EC2 Instance State-change Notification",
   "source":"aws.ec2",
   "account":"123456789012",
   "time":"2019-11-11T21:29:54Z",
   "region":"us-east-1",
   "resources":[
      "arn:aws:ec2:us-east-1:123456789012:instance/i-abcd1111"
   ],
   "detail":{
      "instance-id":"i-abcd1111",
      "state":"pending"
   }
}
```

It should look something like this:

![example EC2 state change](./example-ec2-state-change.png)

### Sending a sample event

* Click the ![Invoke button](./invoke.png) button to send the event.

* Verify in you Panther console that the event was received (it may take a couple of seconds)

![EC2 event arrived in Panther](./example-ec2-in-panther.png)

* ![Invoke button](./invoke.png) it a couple more times and the `tally` in Panther has increased:

![EC2 event de-duplicated](./example-ec2-in-panther-incremented.png)
