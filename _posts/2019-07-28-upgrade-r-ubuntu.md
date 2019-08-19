---
layout:     post
title:      "Upgrade R to 3.5+ on Ubuntu"
date:       2019-07-28 12:00:00
author:     "Jason Feng"
header-img: "img/railway-track-2049394_960_720.jpg"
excerpt_separator: <!--more-->
tags:
    - Ubuntu
    - R
---

I need to upgrade my current R 3.4 to R 3.5+ on Ubuntu 16.04. Here is how.
<!--more-->

#### Step 1: Add the repo
Add the following to `/etc/apt/sources.list` to obtain the latest R 3.6 packages.
```bash
deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran35/
deb http://cran.rstudio.org/bin/linux/ubuntu xenial-cran35/
```

#### Step 2: Upgrade to R 3.6
Run the following commands to install the R
```bash
sudo apt-get update
sudo apt-get install r-base
```
Users who need to compile R packages from source [e.g. package maintainers, or anyone installing packages with install.packages()] should also install the r-base-dev package:
```bash
sudo apt-get install r-base-dev
```

#### Step 3: Upgrade to specific R version (Optional)
If you want to upgrade to a specific version, run
```bash
apt-cache showpkg r-base 
```
The output is something like 
```bash
Package: r-base
Versions: 
3.6.1-3xenial (/var/lib/apt/lists/cloud.r-project.org_bin_linux_ubuntu_xenial-cran35_Packages) (/var/lib/apt/lists/cran.rstudio.org_bin_linux_ubuntu_xenial-cran35_Packages) (/var/lib/dpkg/status)
 Description Language: 
                 File: /var/lib/apt/lists/au.archive.ubuntu.com_ubuntu_dists_xenial_universe_binary-amd64_Packages
                  MD5: 5787ca79ed716232c4cc2087ed9b425b
```
Then run
```bash
 sudo apt-get install -f r-base=3.6.1-3xenial
 ```                  

**Ref**
- [Ubuntu packages for R](https://cran.r-project.org/bin/linux/ubuntu/)
- *[Installing R and Rstudio on Ubuntu Linux](https://blog.zenggyu.com/en/post/2018-01-29/installing-r-r-packages-e-g-tidyverse-and-rstudio-on-ubuntu-linux/)*