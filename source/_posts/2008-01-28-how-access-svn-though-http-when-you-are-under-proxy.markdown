---
layout: post
title: "How access SVN though HTTP when you are under proxy"
date: 2008-08-28 10:14
comments: true
categories: [VCS, SVN]
---

You need to configure subversion "server" file
You can find it:

At windows platform

at Linux based platform
If you want common proxy setting for all SVN servers
Find and fix this group
{% codeblock %}
[global]
http-proxy-host=proxyhost
http-proxy-port=3128
{% endcodeblock %}
If you want separed proxy setting for each svn server

Find group called {% codeblock %}[groups].{% endcodeblock %}

Add new line
{% codeblock %} groupName = {% endcodeblock %}

Add new group with name groupName (previously added)
{% codeblock %}
[groupName]
http-proxy-host = proxy1.some-domain-name.com
http-proxy-port = 80
http-proxy-username = blah
http-proxy-password = doubleblah
http-timeout = 60
neon-debug-mask = 130
{% endcodeblock %}

That's all, just save your file and use it
