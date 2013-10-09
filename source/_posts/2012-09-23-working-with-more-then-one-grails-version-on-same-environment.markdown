---
layout: post
title: "Working with more then one grails version on same environment"
date: 2012-09-23 19:04
comments: true
categories: Grails
---

Typically you have your grails installed into some directory, created environment variable GRAILS_HOME and you are ready to go.

But what if you have few projects with different grails versions? You can have different reasons for that, but nevertheless you need it.

Here is short tip how this process can be simplified, example for Linux (Ubuntu).
Usually I install all applications to /usr/local/. Let's try to work with two grails versions 2.0.0 and 2.1.1(latest for this period)
So after unpacking we have
{% codeblock lang:sh %}

/usr/local/grails-2.0.0
/usr/local/grails-2.1.1
{% endcodeblock %}
Let's create link to any version of Grails
{% codeblock lang:sh %}
>ln -s /usr/local/grails-2.1.1 grails
{% endcodeblock %}
Now we have
{% codeblock lang:sh %}
/usr/local$ ls -ld grails*
lrwxrwxrwx &nbsp;1 root root &nbsp; 23 Sep 23 15:05 grails -&gt; /usr/local/grails-2.1.1
drwxr-xr-x 12 root root 4096 Dec 15 &nbsp;2011 grails-2.0.0
drwxr-xr-x 13 root root 4096 Sep 12 10:30 grails-2.1.1
{% endcodeblock %}

and variable

{% codeblock lang:sh %}
echo $GRAILS_HOME
/usr/local/grails
{% endcodeblock %}
Let's use simple script to change grails version. Mainly the only thing we have to do is to reassign link /usr/local/grails to version we would like to use
{% codeblock lang:sh %}
#!/bin/bash
#Script for changing grails version

grailsVersion=$1
rootPath="/usr/local"
grailsLinkPath=$rootPath"/grails"
grailsPath=$grailsLinkPath"-"$grailsVersion

echo stitching to version $1
#Check if directory with new grails version exists before doing anything
[ -d $grailsPath ] &amp;&amp; rm $grailsLinkPath &amp;&amp; ln -s $grailsPath $grailsLinkPath &amp;&amp; echo "version switched to "$grailsVersion || echo 'Directory '$grailsPath' not found'
{% endcodeblock %}

