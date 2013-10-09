---
layout: post
title: "Deleting grails domain entity without fetching it"
date: 2012-10-24 15:52
comments: true
categories: [Groovy, Grails]
---

So many times I've seen how people working with Hibernate and complaining how slow it is. What I actually see, is how people do not care about tools that they are using. They have strange assumption if tool supposed to their life easier then it supposes to 100% easier and no reason to read posts about such tool or investigate how it can be configured.
Here is only on tip how to avoid useless selects hibernate or Grails GORM. Imaging you have an application that manipulates some data, lets say Comments. There is also controller or some API to delete one comment. If request comes from somewhere outside you application usually it looks like delete something with id=1. http://localhost/comments/delete/1
Implementation of controller usually looks like
{% codeblock lang:groovy %}
    def deleteComment() {
        def commentId = params.long('id')
        def comment = Comment.findById(commentId)
        comment?.delete()
    }
{% endcodeblock %}
So bad part on this example is that we fetch from database entity that we do not need at all. We want to delete it. Hibernate and so GORM are working objects and not with parts of its fields. But it doesn't mean that you cannot do it, just use HQL. Here is simple HQL that will generates pure delete SQL query.
Comments.executeUpdate("delete from Comments where id = :id", [id:commentId])
Of cause you would like to write such code whenever you will need to delete entity, so we will add dynamic method to each Domain class in our application. Please add next code to your BootStrap.groovy
{% codeblock lang:groovy %}
    def grailsApplication

    def init = { servletContext ->
        grailsApplication.domainClasses.each {def domain ->
            domain.metaClass.static.deleteById = {def id ->
                executeUpdate("delete from ${domain.name} where id = :id", [id:id])
            }
        }
    }
{% endcodeblock %}
Now each of your domain class has the method deleteById, and our example became
{% codeblock lang:groovy %}
    def deleteComment() {
        def commentId = params.long('id')
        Comment.deleteById(commentId)
    }
{% endcodeblock %}
You can use this Grails plugin https://github.com/grygoriy/grails_gorm_utils that adds this method for you. Enjoy!
