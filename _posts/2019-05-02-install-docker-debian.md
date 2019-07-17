---
layout:     post
title:      "Install Docker on Debian/Ubuntu"
date:       2019-05-02 12:00:00
author:     "Jason Feng"
header-img: "img/post-bg-2015.jpg"
mathjax: true
excerpt_separator: <!--more-->
tags:
    - Docker
    - Debian
    - Ubuntu
---

A quick reference for myself regarding the steps to install Docker on Debian/Ubuntu.

<!--more-->

## Install docker on Debian

### Install dependencies for docker
```
sudo apt-get -y install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common 
```
### Install Docker's official GPG key
```
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
```

### Check that the GPG fingerprint of the Docker key you downloaded matches the expected value
```
sudo apt-key fingerprint 0EBFCD88
```

### Add the stable Docker repository
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")    $(lsb_release -cs) stable"
```

### Install docker
```
sudo apt-get update
sudo apt-get -y install docker-ce
```

### Configure Docker to run as a non-root user
```
sudo usermod -aG docker $USER
```

### Exit the shell and SSH to the VM again

### Check docker is installed correctly and confirm that you can run Docker as a non-root user
``` 
docker run hello-world
```