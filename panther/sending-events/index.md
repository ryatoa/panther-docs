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

This page provides a guide to the configuration required once the
logging software has been installed. (Please note that the
instructions here are based on clean installations of the logging
software -- if site-specific configurations have already been made,
then it is necessary to download the Panther resources and integrate
them following the providers' documentation.)

All users should read the general advice in the introduction, but then
refer to the relevant sections for their own specific software.

 * [Introduction (all systems)](#introduction)
 * [Download configuration archive (all systems)](#downloading-configuration-archive)
 * [Rsyslog (Linux)](./rsyslog.md#rsyslog-configuration)
 * [NXLog](./nxlog.md) ([Linux](./nxlog.md#nxlog-configuration-linux) \| [Windows](./nxlog.md#nxlog-configuration-windows))
 * [AWS](./aws.md)

# Introduction

Event data is sent securely to the Panther server from local clients
via an encrypted connection using Transport Layer Security (TLS). This
requires the use of certificates and unique client keys which are
generated specifically for your Panther instance during the sign-up
process.

Since these certificates and keys are needed to configure client event
loggers, they are bundled into "configuration archives" along with
sample configuration files specific to the software, and made
available for download by Panther.

You should ensure that the `client.key` included in your configuration
archive is kept securely to prevent its use by anyone else.

The configuration process therefore is to download an appropriate
archive, to load it in a suitable location for the software, and to
carry out any remaining package or system specific tasks.


