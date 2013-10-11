---
layout: post
title: "How to debug tests maven test via Intellij Idea"
date: 2011-01-20 23:11
comments: true
categories: [Intellij, java]
---
Not always you can run tests via IDE. Sometimes configs are complected and it is much easier just to type
<pre> $ mvn test </pre>

If you want to debug them, you can connect via remote.

1. run you test
<pre>$ mvn test -Dmaven.surefire.debug </pre>
maven will prepare environment and will wait for remote connection
2. connect via Idea
{% img /images/2011-01-20-how-to-debug-tests-maven-test-via-intellij-idea/01.png %}
3. start debug <br/>
<p>
{% img /images/2011-01-20-how-to-debug-tests-maven-test-via-intellij-idea/02.png %}
</p>
4. Now test will stop at your breakpoints. Profit!
