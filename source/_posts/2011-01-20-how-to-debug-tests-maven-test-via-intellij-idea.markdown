---
layout: post
title: "How to debug tests maven test via Intellij Idea"
date: 2011-01-20 23:11
comments: true
categories: [Intellij, java]
---
Not always you can run tests via IDE. Sometimes configs are complected and it is much easier just to type
<pre> $ mvn test </pre>

If you want to debug them, you can connect via remote. <br/>
<!--more-->

* run you test <br/>
<pre>$ mvn test -Dmaven.surefire.debug </pre>
maven will prepare environment and will wait for remote connection <br/>

* connect via Idea <br/>

{% img https://lh5.googleusercontent.com/-d2GmgIvfhOQ/TxkmO-2ErvI/AAAAAAAAFz0/LduWaxnFOpk/w694-h592-no/Untitled.png %} <br/>

* start debug <br/>

{% img https://lh3.googleusercontent.com/-Xe-YoYmMITg/Txkm1OSUWYI/AAAAAAAAFz8/LKBvgNDOAUM/w219-h53-no/run.png %}<br/>

* Now test will stop at your breakpoints. Profit!
