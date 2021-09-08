---
title: Rule of Snort rule
---

Snort and Suricata use same language and structure of their rules. Different
about that is option provided of both and feature provided. For example, Snort
don't have spesific rule option for HTTP Header just general purpose, but Suricata
have more spesific HTTP Header for each purpose like HTTP User Agent, HTTP Method,
etc.

## Motivation

While learning for Preparing LKS Province, Material very exacited for me is IDS.
Because I can explore Intrusion Detection System tools, like Suricata and Snort.
Have a few Challenge in Installing and Writing rules for IDS, like you should
know how to manually compile a software from source, and Writing rules for IDS
with minimal false positive result. And now i wanna share with you what I learn
in writing rule with Snort and Suricata.

# Snort rules

Snort have 2 part of rules, first Rule Header and second is Rule Option. below is
example of snort rules.

![Snort rules](/assets/images/snort-rules.png)

## Rule Header

![Snort Rule Header](/assets/images/rule-header-snort.png)


