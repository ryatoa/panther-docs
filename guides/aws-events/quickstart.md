---
layout: home
title: AWS events (quickstart)
nav_order: 2
permalink: /guides/aws-events-quickstart
layout: template
description: AWS events to Panther
parent: Panther guides
image: /img/panther_logo_thin_with_aws.png
---

# AWS-Events2Panther

This guide will walk you through deploying an AWS Lambda function to collect event data and send to Panther via the HTTP API.


# Installation

The quick installation assumes you are familiar with using AWS from the command line, with the necessary tools installed, and credentials.  In short you will need `aws-sam-cli` which can be installed with:

```
pip3 install aws-sam-cli
```


## Build and Deploy to AWS

During the deployment you will be prompted for the following values:

|`APIToken`| Panther [HTTP API Key](../panther/../../panther/admin/index.md#ap-keys) this will be a long 32 character string of random letters and numbers |
|`ConsoleFQDN`| The fully qualified name of your Panther console e.g. [example.app.panther.support](https://app.panther.support){:target="_blank"} or self hosted hostname |


From a `git clone` of [panther-aws-events](https://github.com/OpenAnswers/panther-aws-events){:target="_blank"} the following commands will deploy panther-aws-events.

* ```
  git clone https://github.com/OpenAnswers/panther-aws-events.git
  cd panther-aws-events
 
  sam build
  sam deploy --guided
  ```

For more information on setting up the AWS command line utilities, please refer to the [AWS events (detailed)](./in-detail.md) guide.

Further instructions for how to send some test events to Panther are on the [AWS events testing](./testing.md) page.
