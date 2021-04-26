---
layout: home
title: Rules file (yaml)
nav_order: 6
permalink: /panther/rules/yaml
parent: Rules
layout: template
description: Panther Rules - yaml definitions
---

# Rules location
Depending on which way you installed Panther there are differing locations of the rule files.

## When using [app.panther.support](https://app.panther.support){:target="_blank"}
  The rules are not directly accessible to the end user but they can be exported, edited and re-imported from `Rules` tab.


## When using [docker-compose.yml](https://github.com/OpenAnswers/panther-core/blob/master/docker-compose.yml){:target="_blank"}
  A docker volume will have been created, the name is comprised of two parts.
  
  `<directory-name>_rules_vol`

  For example if your `docker-compose.yml` is in a directory called `panther` then the docker volume will be called
  `panther_rules-vol`

  e.g.
  ```console
  [root@localhost ~]# ls -l /var/lib/docker/volumes/panther-core_rules-vol/_data/
  total 14
  -rw-r--r-- 1 ansible ansible  588 Jul  7  2020 http.rules.yml
  -rw-r--r-- 1 ansible ansible  860 Jul  7  2020 server.rules.yml
  -rw-r--r-- 1 ansible ansible 2256 Jul  7  2020 syslogd.rules.yml
  ```


## When running the [source code](https://github.com/OpenAnswers/panther-core){:target="_blank"}
  The rules are located under `rules/`

  e.g.
  ```console
  [root@localhost panther-core]# ls -l rules/
  total 23
  -rw-r--r-- 1 root root  588 Feb 23 11:36 http.rules.yml
  -rw-r--r-- 1 root root  815 Mar 26 16:12 server.rules.yml
  -rw-r--r-- 1 root root 2256 Feb 23 11:36 syslogd.rules.yml
  ```


# Rules format

The `server.rules.yml` has three main sections

```yaml
globals:

groups:

schedules:
```
  
