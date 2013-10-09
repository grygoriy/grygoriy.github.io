---
layout: post
title: "Grails at Ubuntu rep"
date: 2011-08-20 16:51
comments: true
categories: [Groovy, Grails]
---
Maybe it is not new to anyone, it became pleasant surprise to me that I don't have to download Grails manually any more. Now I use "Ubuntu  way". 

First of all we need to add Grails repo

{% codeblock %}
sudo add-apt-repository ppa:groovy-dev/grails
sudo apt-get update
{% endcodeblock %}
That's all,  now you can install latest stable release of Grails
{% codeblock %}
sudo apt-get install grails
{% endcodeblock %}

The thing I'm exiting a lot is that you can install any version of Grails. Usually you don't migrate to new version right after it has been released.

So here is list of available Grails versions
sudo aptitude search grails <br/>
p grails - A rapid web development platform built on Groovy <br/>
p grails-1.2.5 - A rapid web development platform on Groovy <br/>
p grails-1.3.0 - A rapid web development platform on Groovy <br/>
p grails-1.3.1 - A rapid web development platform on Groovy <br/>
p grails-1.3.2 - A rapid web development platform on Groovy <br/>
p grails-1.3.3 - A rapid web development platform on Groovy <br/>
p grails-1.3.4 - A rapid web development platform on Groovy <br/>
p grails-1.3.5 - A rapid web development platform on Groovy <br/>
p grails-1.3.6 - A rapid web development platform on Groovy <br/>
p grails-1.3.7 - A rapid web development platform on Groovy <br/>
p grails-1.4.0 - A rapid web development platform on Groovy <br/>
p grails-2.0.0 - A rapid web development platform on Groovy <br/>
p grails-bash-completion - Provides bash autocompletion for  <br/>
Grails, a rapid web development platform built on Groovy
