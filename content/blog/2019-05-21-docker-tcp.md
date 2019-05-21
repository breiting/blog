---
title: "Enable tcp access for docker"
date: 2019-05-21
tags: ['docker']
---

On my Arch system, docker does not expose the tcp port by default, therefore I get the following error:

```
Warning: failed to get default registry endpoint from daemon
(Cannot connect to the Docker daemon at tcp://192.168.0.16:2375. Is the docker daemon running?).
Using system default: https://index.docker.io/v1/
Cannot connect to the Docker daemon at tcp://192.168.0.16:2375. Is the docker daemon running?
```

It took me quite some time to get the root cause of the problem. An easy fix is to enable the tcp port in the docker
service configuration file `/lib/systemd/system/docker.service`.

Just replace the section:

```
[Service]
ExecStart=/usr/bin/dockerd -H fd://
```

by

```
[Service]
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```

Make sure to reload the daemon `systemctl daemon-reload` and restart the docker service `systemctl restart docker`. This
should fix the problem.
