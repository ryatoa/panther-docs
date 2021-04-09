---
layout: home
title: Sending Events to Panther
nav_order: 3
permalink: /panther/config
layout: template
description: Panther make it easy to ingest events by providing downloadable pre-configured configuration files for popular logging software such as Ryslog and NXLog
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
 * [Download configuration archive (all systems)](#rsyslog-configuration)
 * [Rsyslog (linux)](#rsyslog-linux)
 * [NXLog (linux)](#nxlog-linux)
 * [NXLog (windows)](#nxlog-windows)

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

<a name="download" id="download"></a>
## Downloading Configuration Archive 

The configuration archives are available on the `Admin` page. Select
this using the tab from the main menu at the top of the Panther page.

![Select the admin page](./media/console-admin-page.png)

The drop-down selector offers archives for the supported software and
operating systems.

![Download the configuration archive](./media/console-admin-download.png)

Select the appropriate option for your client and click the `Download`
button.

The archives are downloaded in an appropriate format for each system:
Linux archives are provided in uncompressed `tar` format, while
Windows archives are built using `zip`.

(Note that the same certificates and keys are provided in each
archive, so for custom configurations the choice of download is
unimportant.)

<a name="rsyslog-linux" id="rsyslog-linux"></a>

# Rsyslog Configuration

For complete information on configuring [rsyslog](https://www.rsyslog.com/),
please consult the official [guides for rsyslog.](https://www.rsyslog.com/category/guides-for-rsyslog/)

Major distributions include rsyslog in their standard repositories, so
please either follow your system's guidelines to install and enable
it, or [visit the official website.](https://www.rsyslog.com/)

Once rsyslog is installed, the necessary steps to integrate it with
Panther are:

 * [install supporting gnutls packages](#rl-gnutls)
 * [install configuration files](#rl-install)
 * [restart rsyslog](#rl-restart)

<a name="gnutls" id="rl-gnutls"></a>

## Install GNU-TLS support for rsyslog

On GNU/Linux systems, rsyslog uses [the GnuTLS](https://gnutls.org/)
library to provide its TLS functionality. This is classed as an
optional dependency on some distributions, and is therefore not always
installed automatically by the package manager. Where this is the
case, the TLS package may have to be added explicitly along with the
main rsyslog package.

### Debian / Ubuntu

```console
# apt-get install rsyslog-gnutls
```

### RedHat / CentOS

```console
# yum install rsyslog-gnutls
```
<a name="rl-install" id="rl-install"></a>

## Install the configuration files

The downloaded resources in `rsyslog-config-linux.tar` need to be
installed into the system rsyslog configuration directory.

The following commands will extract the archive into
`/etc/rsyslog-client.d`, creating it if necessary. If the directory
already exists, then make a backup of it before extracting the archive
in case any previously configured files are overwritten.

```console
[root@localhost ~]# cd /etc
[root@localhost /etc]# tar -xvf /path/to/rsyslog-config-linux.tar
```

## Edit the main rsyslog configuration

Once the new configuration files have been installed, the main system
rsyslog configuration file must be updated in order to reference them.

Edit `/etc/rsyslog.conf` and make sure that it contains a line like
this to read the Panther configuration files extracted from the
archive above:

```console
$IncludeConfig /etc/rsyslog-client.d/*.conf
```

<a name="restart" id="rl-restart"></a>

## Restart rsyslog

The new configuration should take effect automatically when the system
is next rebooted, but it is also possible to restart rsyslog to
have the changes take effect immediately.

### systemd based systems

Systems using the newer systemd service manager, such as Ubuntu 18 and
CentOS 7, can restart the rsyslog service as follows:

```console
# systemctl restart rsyslog
```

### init.d based systems

Systems using the older System V init service management scripts can
generally restart the rsyslog service as follows:

```console
# /etc/init.d/rsyslog restart
```
<a name="nxlog-windows" id="nxlog-linux"></a>

# NXLog Configuration (Linux)

Install the NXLog Community Edition
[from the official website](https://nxlog.co/products/nxlog-community-edition/download)
following
[the deployment guide.](https://nxlog.co/documentation/nxlog-user-guide/deployment.html)

Once installed, the steps to integrate NXLog with Panther are:

 * [install configuration files](#nl-install)
 * [restart nxlog](#nl-restart)

<a name="nl-install" id="nl-install"></a>

## Install the configuration files

To begin with, download the configuration archive
[as described above.](#download-configuration-archive)
The downloaded resources in `nxlog-config-linux.tar` then need to be
installed into the system NXLog configuration directory.

The following commands will extract the archive into `/etc/nxlog`,
which should have been created during the NXLog installation.  Note
that any existing `nxlog.conf` will be overwritten, so make a backup
if one already exists.

```console
[root@localhost ~]# cd /etc/nxlog
[root@localhost /etc/nxlog]# tar -xvf /path/to/nxlog-config-linux.tar
```

Apart from overwriting the main `nxlog.conf`, this will extract the
required certificates and key files into a new `panther` sub-directory.

<a name="nl-restart" id="nl-restart"></a>

## Restart NXLog

If NXLog has been installed according to
[the deployment guide,](https://nxlog.co/documentation/nxlog-user-guide/deployment.html)
then it should restart automatically following a system reboot.

In order to have the new configuration take immediate effect, it is
also possible to restart NXLog immediately. While systemd systems such
as Ubuntu and CentOS are supported, at the time of writing the service
management interface for NXLog is implemented as a wrapper around the
System V init scripts.

This means that, on both systemd and init based systems, it is
possible to restart NXLog by calling the init script directly:

```console
[root@localhost /etc/nxlog]# /etc/init.d/nxlog restart

```
<a name="nxlog-windows" id="nxlog-windows"></a>

# NXLog Configuration (Windows)

Download the Windows installer for the NXLog Community Edition
[from the official website.](https://nxlog.co/products/nxlog-community-edition/download)

Install the package by running the installer, but also see
[the deployment guide.](https://nxlog.co/documentation/nxlog-user-guide/deployment.html)

Once installed, the steps to integrate NXLog with Panther are:

 * [install configuration files](#nw-install)
 * [restart nxlog](#nw-restart)

<a name="nw-install" id="nw-install"></a>

## Install the configuration files

Once the Windows configuration archive has been downloaded
[as described above,](#download-configuration-archive) the contents of
`nxlog-config-windows.zip` must be overlaid onto the base directory of
the installed NXLog package.  If the default install options were
used, this will normally be `C:\Program Files (x86)\nxlog`.

Opening this directory with the Windows File Explorer will show that it
contains `cert` and `conf` sub-directories, among others. While the
`cert` directory should be empty, the `conf` directory may contain a
sample `nxlog.conf`, which should be renamed `nxlog.dist` in order to
prevent any clash with the new configuration.

Extract the downloaded `nxlog-config-windows.zip` Panther
configuration archive by clicking on it and selecting the `extract to`
option, specifying the base directory NXLog was installed into --
normally `C:\Program Files\nxlog` -- containing the `cert` and `conf`
sub-directories.

This will copy the certificates and key files into the empty `cert`
directory, along with a new `nxlog.conf` into the `conf`
directory. Note that the system may ask for confirmation when writing
to the `cert` directory, owing to its permissions.

<a name="nw-restart" id="nw-restart"></a>

## Restart NXLog

The NXLog installer should have configured the service to be started
automatically on the next system boot.

From the NXLog documentation, it is also possible to start the service
directly from the services console, or from the command line interface
by navigating to the install directory and running `nxlog.exe -f`.
This last option may assist in initial debugging.
