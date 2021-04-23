---
layout: home
title: Rules
nav_order: 6
permalink: /panther/rules
has_children: true
layout: template
description: Panther Rules - processing
---


# Overview

Rules control how log messages are turned into events. They can be
used to discard, de-duplicate or modify the events that appear in the
Panther console.

Events are received from an external source, processed by user defined
rules, and finally stored on the Panther server.

@startuml
cloud "event source" as c1
cloud "Panther" as pan {
  agent "Agent" as mon
  agent "Rules" as rules
  database storage

} 

c1 -right-> mon
mon -right-> rules
rules -right-> storage
@enduml


## Processing order

@startuml
agent "Agent Rules" as a1
agent "Global Rules" as a2
agent "Group Rules" as a3

a1 --> a2
a2 --> a3
@enduml

The set of [Syslog Mappings](#syslog-mappings) are processed first,
followed by the [Global Rules](./global.md#global-rules) and, finally, any optional
[Groups Rules](./group.md#group-rules).




# Details

  Each rule definition is stored in yaml file.

  A basic rule has a `name` followed by a *Select* verb, such as `match`, and an *Action* verb, such as `discard`:

  ```yaml
    name: 'Rule name'
    match:
      some_field: value
    discard: true
  ```

  Multiple *Select*s are interpreted as logical "and" operations.
  This example is equivalent to "*Select* WHERE `some_field` **matches** the regular expression `/value/` AND where `other_field` **equals** `value`":

  ```yaml
    match:
      some_field: !!js/regexp /value/
    equals:
      other_field: value
  ```

  Multiple *Actions* can be specified too:

  ```yaml
    discard: true
    stop: true
  ```

  A *RuleSet*, which groups the rules into logical areas, is made up of an array of *Rules*. These are processed in order, until the last rule is found or a `stop` or `stop_ruleset` action is encountered:

  ```yaml
    - name: 'Rule name'
      match:
        some_field: value
      set:
        new_field: 17

    - name: 'No junk'
      equals:
        my_field: 'linux'
      set:
        that_field: 'Torvalds'
  ```

  There are multiple *RuleSets* used during processing. The *Global* *RuleSet* is processed first, followed
  by any *Group* rules. Only one group will be matched, based on the *Select* verbs, and its rules then processed.

  *Discard* and *Dedupe* are shortcuts for what end up as rules that are placed before the rest of the *RuleSet*.



## Rule Order

Rules are applied in the following order:

  1. Syslog mapping
  2. Global discards
  3. Global de-duplication
  4. Global rules, in order
  5. Grouping, and the group rules. (group_order: if required? TODO - check)
      1. discard_first
      2. dedupe
      3. rules
      4. discard_last
  6. Discards that rely on other mappings


## Shortcut helpers

  There are some tasks that are repeated frequently, so the most
  reqular have shortcuts set up to make the rules more succinct.

  TODO - not clear how or where to use these!

### `syslog_mapping`

  Takes the syslog levels and maps them
  to console severities.

  * This is a required field (TODO - what is?)

### `deduplicate`

  Shortcut for matching on `summary` with a regex and replacing it.

     - [ /match/, 'replace' ]

  Or supply a second regex if you want a more specific search replace

     - [ /match/, /replace_match/, 'replace' ]

  * A match will short-circuit dedupe execution, but continue on to
  any following rules.

### `discard` / `discard_last`

  Shortcuts for dumping messages, either before or after normal
  processing.

  Discard on summary:

      - '/trash/'

  Or write a full rule

      - name: 'To the bin'
        match:
          summary: /trash/

  * Discard will short-circuit execution.

