---
layout: post
title: "How to run Grails application on separate port"
date: 2012-02-06 18:30
comments: true
alias: /2012/02/how-to-run-grails-application-on.html
categories: [Groovy, Grails]
---
Some times you need to run your Grails app on different port and different context during development. Typical example when you are developing application that is divided into two or more apps (Services or other) So one application will run on 8080 and other for example 8081. That's allows you to run both applications in same time, work with them and debug

So we can change port with command <br/>
__grails run-app -Dserver.port=8081__

But it is not very convenient to do it all the time, so I prefer to change it in BuildConfig.groovy, just put next line somewhere in file <br/>
__grails.server.port.http="8081"__

and to change running context (default is localhost:8080/appname) add app.context= to application.properties. Next line will run Grails application at root context <br/>
__app.context=/__
