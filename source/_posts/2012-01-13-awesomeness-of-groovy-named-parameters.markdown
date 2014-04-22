---
layout: post
title: "Awesomeness of groovy named parameters"
date: 2012-02-13 18:33
comments: true
alias: /2012/01/awesomeness-of-groovy-names-parameters.html
categories: [Groovy, DSL]
---

How do you call typical method with few parameters. Lets assume we have method that sends email with signature<br />
{% codeblock lang:groovy %}
send(String from, String to, String subject, String body) {
  println "sender ${from}"
  println "sender ${to}"
  println "sender ${subject}"
  println "sender ${body}"
}
{% endcodeblock %}
typical java call would be
{% codeblock lang:groovy %}
send("john@example.com", "mike@example.com", "greetings", "Hello Goodbye")
{% endcodeblock %}

Too many strings, don't you think? I have to check java docs all the time I'm writing this method.<br />
Groovy allows us to pass parameters as map without []. If we pass map into method groovy set it to the first parameter. Let's look into other exampl

{% codeblock lang:groovy %}
send from:"john@example.com",
     to:"mike@example.com",
     subject:"greetings",
     body:"Hello Goodbye"
{% endcodeblock %}
Much more readable as for me. And method will looks like
{% codeblock lang:groovy %}
send(params) {
  println "sender ${params.from}"
  println "sender ${params.to}"
  println "sender ${params.subject}"
  println "sender ${params.body}"
}
{% endcodeblock %}

