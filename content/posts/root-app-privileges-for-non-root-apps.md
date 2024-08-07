---
layout: post
title:  "[Linux] Granting a non root app access to priviliged ports"
date:   2024-02-26
authors: ["Elias Bourgess"]
slug: "root-app-privileges-for-non-root-apps"
categories: ["DevOps", "Linux"]
draft: false
---

As we already know, ports up until `1024` are only accessible by root processes. Therefore, in order to be able to run any process on the ports below `1024` the following command should be run.

```bash
sudo setcap cap_net_bind_service=+ep /path/to/application
```

Keep in mind that these restrictions are in place for a reason, and having a reverse proxy would be a better solution here. However there are some extreme cases when this is actually needed.
