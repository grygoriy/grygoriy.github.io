---
layout: post
title: "use Grails Domain with string type id"
date: 2012-01-03 18:41
comments: true
alias: /2012/01/use-grails-domain-with-string-type-id.html
categories: [Grails, Groovy]
---
Some times you need id with different type then Long. It doesn’t metter you have lagacy database or it requirement from your DBA.

In general all you need is described in official docs [http://grails.org/doc/2.0.x/ref/Database Mapping/id.html](http://grails.org/doc/2.0.x/ref/Database%20Mapping/id.html)

But there is at least one case with very strange behaviour. When we define key as String type

{% codeblock lang:groovy %}
Foo {
 static mapping = {
   id name: 'code', column: 'code', type: 'string'
 }
}

{% endcodeblock %}

For the first look it works fine Foo.get(‘someKey’)
returns exact object I was looking. But when we try to use string that looks like a number

{% codeblock lang:groovy %}
Foo.get('4’)
{% endcodeblock %}
we will get

{% codeblock lang:groovy %}
org.hibernate.TypeMismatchException
{% endcodeblock %}

Provided id of the wrong type for class Foo. Expected: class java.lang.String, got class java.lang.Long
It looks like id field is still Long and grails first of all trying to cast serializable to Long and only after that it looks for type property.
Workaround is to add explicitly id field to domain class

{% codeblock lang:groovy %}
Foo {
  String id
     
  static mapping = {
    id name: 'code', column: 'code', type: 'string'
  }
}
{% endcodeblock %}

It is mentioned in documentation but now grails knows what type to use.
There is issue in grails jira which was closed with resolution Not A Bug.
