---
author: Ahmad Bayhaqi
pubDatetime: 2021-09-08
title: "Create Snort rules"
postSlug: create-snort-rules
featured: false
draft: false
tags:
  - snort
  - ids
ogImage: ""
description: ""
---

## Table of contents

## Intro

Snort and Suricata use the same language and structure of their rules. Different
about that is an option provided of both and feature provided. For example, Snort
don't have a specific rule option for HTTP Header just general-purpose, but Suricata
have more specific HTTP Header for each purpose like HTTP User-Agent, HTTP Method,
etc.

## Motivation

While learning for Preparing LKS Province, the Material very excited for me is IDS.
Because I can explore Intrusion Detection System tools, like Suricata and Snort.
Have a few Challenges in Installing and Writing rules for IDS, like you should
know how to manually compile software from source, and Writing rules for IDS
with a minimal false-positive result. And now I wanna share with you what I learn
in writing rules with Snort and Suricata.

## Snort rules

Snort has 2 parts of rules, the first is Rule Header and the second is Rule Option. below is
example of snort rules.

![Snort rules](/assets/images/snort-rules.png)

## Rule Header

Rule Header contains the information that defines the who, where and what of packet, as well as what to do in the event that a packet with all the attributes indicated in the rule should show up.

![Snort Rule Header](/assets/images/rule-header-snort.png)

### actions

actions used for notice snort what should action if found packet with rule database.
snort have 3 in IDS mode, like alert, log, and pass. but if you running snort "inline mode" or IPS
you have 3 additional action option, like drop, reject, and sdrop.

| actions  | Function                                                                                                   |
| -------- | ---------------------------------------------------------------------------------------------------------- |
| `alert`  | generating alert using selected alert method, and log packet.                                              |
| `log`    | Just log it.                                                                                               |
| `pass`   | ignore packet                                                                                              |
| `drop`   | block and log packet                                                                                       |
| `reject` | block, log it, and send TCP reset if protocol is TCP and ICMP port unreachable message if protocol is UDP. |
| `sdrop`  | block packet but don't create log                                                                          |

### Protocol

available protocol in snort,

| Protocol | Function |
| -------- | -------- |
| TCP      |          |
| ICMP     |          |
| UDP      |          |

### IP Address

you can define with variable, CIDR block, or you can use all (any) keywords for
all IP addresses.

|                     | Example                         |
| ------------------- | ------------------------------- |
| Variable            | `ipvar MY_NET 102.159.23.2`     |
| Single IP Address   | `192.168.0.1`                   |
| CIDR block          | `192.168.0.0/24`                |
| IP List             | `[10.10.10.10, 192.168.0.0/24]` |
| Single IP Negation  | `!192.168.0.1`                  |
| Negation CIDR block | `!192.168.0.0/24`               |
| Negation IP List    | `![10.10.10.0/24, 172.16.0.1]`  |
| Any IP Addresses    | `any`                           |

### Port

same as IP Address you can define it with variable, and also define any port, static port, range port, and negation port

|                            | Example                              |
| -------------------------- | ------------------------------------ |
| Variable                   | `portvar MY_PORTS [22,80,3028:4028]` |
| Static port                | `80`                                 |
| port 1 - 1024              | `any`                                |
| Less than or equal to (<=) | `:1024`                              |
| Ignore static port         | `!80`                                |
| Ignore port range          | `!600:610`                           |
| Port range                 | `100:200`                            |

### Direction

in snort have 2 Direction "->" and "<>". Direction operator mean considered to be the traffic coming from the source host, and the address and port information on the right side of the operator is the destination host.

| Operator | Function                            |
| -------- | ----------------------------------- |
| `->`     | source to destination               |
| `<>`     | source to destination or vice versa |

## Rule Options

![Snort Rule Option](/assets/images/rule-option-snort.png)

Rule options form the heart of Snort's intrusion detection engine, combining ease of use with power and flexibility. All Snort rule options are separated from each other using the semicolon (;) character. Rule option keywords are separated from their arguments with a colon (:) character.

Snort have 4 category of rule options, for each category have different purpose. like

- General Rules
- Payload
- Non-Payload
- Post Detection

### General Rules

| option      | functions                                                |
| ----------- | -------------------------------------------------------- |
| `msg`       | message to print                                         |
| `reference` | reference to help identify an attack                     |
| `gid`       | Group id, by default `1` (advance user)                  |
| `sid`       | uniquely number to easily identified rules               |
| `rev`       | revision number                                          |
| `classtype` | categorize rules detected attack                         |
| `priority`  | severity level to rules                                  |
| `metadata`  | metadata for rules, in dictionary format, key and values |
