---
layout: post
title: "ubuntu 11.04 to create usb stick for ubuntu 10.04"
date: 2011-06-06 10:00
comments: true
categories: Ubuntu
---
Threre is crazy bug if you are trying to use Startup Disk Creator form Ubuntu 11.04 to create startup usb for Ubuntu 10.04.
At startup you will got nice error

<pre>vesamenu.c32: not a COM32R image
boot:
vesamenu.c32: not a COM32R image
boot:
(...)</pre>

To be honest that tells nothing to me. This comes from backward compatibility so if you want USB stick with ubuntu 10.04 you have to have same version of ubuntu.

Here is pretty strange workaround that works for me. Just create usb stick with any version of StartupDiskCreator, when you got error message type "help" in prompt. At help menu hit enter, system should run.

Workaround is more then strange, but it works
