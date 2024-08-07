---
layout: post
title:  "[Git] Deleting all Branches but master"
date:   2022-08-19
slug: "deleting-all-branhes-but-master"
authors: ["Elias Bourgess"]
categories: ["DevOps", "GitOps"]
draft: false
---

In order to delete all branches but master simply run the following command: 

```shell
git checkout master
git branch | grep -v '^*' | xargs git branch -d
```
