---
layout: post
title:  "[Git] Migrating from Gitlab to Github"
date:   2022-04-23
authors: ["Elias Bourgess"]
slug: "migrating-from-gitlab-to-github"
categories: ["DevOps", "GitOps"]
tags: ["git", "github-gitlab-migration"]
draft: false
---

When we were migrating from Gitlab to Github, I found out that there wasn't an easy way to import everything from Gitlab to Github, as Github doesn't offer a good tool for that, so I ended up writing the small script below and hosted it as a [gist](https://gist.github.com/ebourgess/ff4d553b55e96b358b7fa1ddc3033ba7) if anyone finds this useful.

```sh
#/bin/sh

DIR=$1
OLDREPO=$2
NEWREPO=$3

# clone the old repository and navigate into the new directory
git clone $OLDREPO $DIR
cd $DIR

# check out all of the remote branches locally
git branch --remotes --no-color | grep --invert-match "\->" | while read remote; do
    git checkout --track "$remote"
done

git checkout master

# set new origin remote url
git remote set-url origin $NEWREPO

# push all branches and tags to the new remote url
git push --mirror $NEWREPO
```