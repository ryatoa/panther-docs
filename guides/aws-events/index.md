---
layout: home
title: AWS events 
nav_order: 1
permalink: /guides/aws-events
layout: template
description: AWS events to Panther
parent: Panther guides
image: /img/panther_logo_thin_with_aws.png
---

# AWS-Events2Panther

This guide will quickly and easily allow a user to deploy a lambda function to collect AWS events and send them to Panther via it's HTTP API.

AWS events are either region specific or global, depending on the service. So to get all events to your panther console you will have to deploy this lambda function to each of your accounts and regions that you wish to monitor.

The NodeJS Javascript code can be changed by you if you wish to support different AWS events, and/or send different data as part of the message.

Events can be sent to either an [app.panther.support](https://app.panther.support) instance or your own self hosted [Dockerised containers](https://hub.docker.com/repository/docker/openanswers/panther-console).


## Checklist

In order to send events from your AWS estate to [Panther](https://app.panther.support) you will need to have the following:

|`APIToken`| Panther [HTTP API Key](../panther/../../panther/admin/index.md#ap-keys) this will be a long 32 character string of random letters and numbers |
|`ConsoleFQDN`| The fully qualified name of your Panther console e.g. [example.app.panther.support](https://app.panther.support) or self hosted hostname |




## System Design

This system has ben designed to gather AWS events, extract data from them and send it to your Panther console.

The general flow of events from AWS to Panther can be sumarised by the following:

@startuml
skinparam backgroundColor #eeeeee

!include <awslib/AWSCommon>
!include <awslib/GroupIcons/all.puml>
!include <awslib/Compute/all.puml>
!include <awslib/ManagementAndGovernance/all.puml>
!include <awslib/SecurityIdentityAndCompliance/all.puml>
!define PantherLogo <img src="https://openanswers.github.io/panther-docs/img/panther_logo_thin.png">

Cloudalt(Cloudalt, "", "AWS") #orange {

  Rectangle "sources" #white {
    GuardDuty(GuardDuty1, "","All events from GuardDuty")
    CloudWatch(CloudWatch1, "","All visible events in region")
  }
  LambdaLambdaFunction(LambdaPantherEvent, "panther-aws-events", "")

  sources -down-> LambdaPantherEvent 
  
}

cloud internet

Rectangle Panther #black [
  PantherLogo as A1
]

LambdaPantherEvent -down-> internet : POST to Panther API (app.panther.support)
internet -down-> Panther 

@enduml


1. The Lambda function is triggered whenever a filter matches an event.
2. It will then format the event data into a Panther json message.
3. Then it will send the message to your Panther console using your private API key.


## Get the code

Download the [panther-aws-events](https://github.com/OpenAnswers/panther-aws-events) code from GitHub with:

  ```console
    git clone https://github.com/OpenAnswers/panther-aws-events.git
  ```

You'll need this before you can install panther-aws-events.

## Installing the code

If you are familiar with AWS and accessing it from the commanline and having a functiong `aws-sam` configured please follow:

  [**AWS Events (quickstart)**](./quickstart.md)

For a more detailed step-by-step approch:

  [**AWS Events (detailed)**](./in-detail.md)

## Running 

Once the code has been successfully registered in AWS you should start seeing a stream of events into Panther.  
Manual testing can be done following the [**AWS Events testing**](./testing.md) instructions.

Further event filtering check [Configuring the captured events](./in-detail.md#3-configuring-the-captured-events)