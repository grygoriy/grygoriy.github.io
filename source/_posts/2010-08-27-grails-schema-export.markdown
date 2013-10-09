---
layout: post
title: "Grails schema-export"
date: 2010-08-27 16:40
comments: true
categories: [Groovy, JVM, Grails]
---

Для себе виявив цікавий скрипт у грейлс
grails schema-export

Власне все що він робить - це генерує скрипт на основі мапінгів доменних класів. Правду кажучи я люблю сам писаті але кілька роз довелось будувати вручну на основі існоючої системи і тоді дуже би знадобився. Але як то кажуть, досвід це та річ яка прихоить коли ти вже все зробив :)

Якщо виконати
grails schema-export
то схема бази даних буде екпортована у target/ddl.sql
Можна екпортувати окремі доменні класи
{% codeblock %}grails schema-export com.User {% endcodeblock %}
