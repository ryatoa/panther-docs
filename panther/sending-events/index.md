---
layout: home
title: Sending Events to Panther
nav_order: 3
permalink: /panther/sending-events
has_children: true
layout: template
description: Panther makes it easy to ingest events by providing downloadable pre-configured configuration files for popular logging software such as Ryslog and NXLog
image: /img/panther-event-sources.jpeg
twitter:
  card: summary_large_image
---

# Overview

The most straightforward way to send events to Panther is to install
and configure compatible logging software on the client system.

While users should generally install the software according to the
documentation published by the relevant providers, specific
configuration is also required to enable communication with Panther.

This and subsequent pages provide a guide to the configuration required once any necessary
logging software has been installed. 

> _Note: The instructions here are based on clean installations of the logging software -- if site-specific configurations have already been made, then it is necessary to download the Panther resources and integrate them following the providers' documentation._

All users should read the general advice in the [introduction](#introduction), but then
refer to the relevant sections for their own specific software.

 * [Introduction](#introduction) (all systems)
 * [Rsyslog](./rsyslog.md#rsyslog-configuration) (Linux)
 * [NXLog](./nxlog.md) ([Linux](./nxlog.md#nxlog-configuration-linux) \| [Windows](./nxlog.md#nxlog-configuration-windows))
 * [Panther API](./panther-api.md) (HTTP)
 * [AWS](./aws.md)

# Introduction

Events can be received by Panther via two protocols:
 
| Protocol | Destination | Port |
| Secure Syslog | example.app.panther.support | 6514 |
| HTTP (Post) | https://app.panther.support | 443 |

These are both _TCP_ ports and may require additional firewalling rules to permit connectivity depending on your networking setup.


## [app.panther.support](https://app.panther.support) (secure syslog)

Event data is sent securely to the Panther server from local clients via an encrypted connection using Transport Layer Security (TLS). This requires the use of certificates and unique client keys which are generated specifically for your Panther instance during the sign-up process. 

> _Note: TLS certificates are used for app.panther.support, self hosted Docker containers use standard Syslog_

Since these certificates and keys are needed to configure client event loggers, they are bundled into "configuration archives" along with sample configuration files specific to the software, and made available for download from your Panther instance e.g. ([example.app.panther.support](https://app.panther.support){:target="_blank"}).

> _Note: You should ensure that the `key.pem` included in your configuration archive is kept secure to prevent its use by anyone else._

The configuration process therefore is to download an appropriate archive, to load it in a suitable location for the software, and to carry out any remaining package or system specific tasks.

There are specific instructions for configuring the following Event senders:

 - [Rsyslog](./rsyslog.md) 
 - [NXLog](./nxlog.md)

## Other Syslog agents 

Any Sylog agent can be used so long as it supports TLS Client certificate authentication, the necessary certificate files can be acquired from the [Rsyslog](./rsyslog.md) configuration archive.

The following files included in the `rsyslog-config-<system>.tar` can be used as the basis for other Syslog agents:

|cert.pem| TLS Client certificate (self signed)|
|key.pem| TLS Client Key|
|panther-cert-chain.pem| The (self signed) Certificate chain of trust |

Syslog events are sent to `app.panther.support:6514` (6514 is the secure syslog port).

## [app.panther.support](https://app.panther.support) (HTTPS API)

Event data is sent securely to the Panther server from local clients via an encrypted HTTPS connection.  This does not require any additional certficates to be installed and will use your systems standard TLS authority chain of trust.

For futher information please consult the general [API Console](../api/index.md) documentation, or the [AWS-Events2Panther](./aws.md). 


## Self Hosted Panther

**TODO**

