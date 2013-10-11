---
layout: post
title: "File Templates in IntellijIdea. Do not repeat yourself"
date: 2013-06-11 23:22
comments: true
published: false
categories: [Intellij]
---

How may times are you creating typical files with pretty common structure? I can say a lot. Most test classes has pretty much same things inside. At least for me this is most visible on tests. Usually things that are repeated are class extension (and I am always forgetting what to extend), static imports for Assert.assertThat() and CommonMatchers. Maybe your also have some test heplers, that you will be adding too. My example will be bases in junit classe for Selenium WebDriver. But this is does not really metters.
So my typical test class usually looks like this:

{% codeblock lang:groovy %}
package com.grygoriy.example.test.imdb.test

import com.grygoriy.example.test.imdb.page.MainPage
import org.junit.Before
import org.junit.Test

import static com.grygoriy.example.test.imdb.util.UITestHelper.at
import static com.grygoriy.example.test.imdb.util.UITestHelper.go
import static org.hamcrest.collection.IsCollectionWithSize.hasSize
import static org.junit.Assert.assertThat

/**
 * User: Grygoriy Mykhalyuno
 * Date: 6/22/13
 * Time: 4:05 PM
 */
class MainPageTest extends AbstractUITest {

    MainPage mainPage;

    @Before
    void init() {
        mainPage = go MainPage
        at mainPage
    }

    @Test
    void MainPageShouldBeInSpecificState() {
        assertThat 'box office has 5 items ', mainPage.boxOfficeWidget.top, hasSize(5)
    }
}
{% endcodeblock %}

This is dummy test that is testing IMDB.com main page. I would like to save its structure as a template. Goto Idea menu Tools->Save File as Template. You should see template 
{% img /images/2013-06-11-file-templates-in-intellijidea-do-not-repeat-yourself/01.png %}

Lets make it looks like a template. First of all I will change template name to UITest. Second template content to
<pre>
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end

import org.junit.*
import static com.grygoriy.example.test.imdb.util.UITestHelper.*
import static org.junit.Assert.assertThat

#parse("File Header.java")
class ${NAME} extends AbstractUITest {
}

</pre>

Documentation about functions you can use in your template you can find in [JetBrains Documentation](http://www.jetbrains.com/idea/webhelp/file-templates.html)
To use this template just press
{% codeblock %} ALT+INSERT {% endcodeblock %}
to create new file and find you template name in drop down.

{% img /images/2013-06-11-file-templates-in-intellijidea-do-not-repeat-yourself/02.png %}

{% img /images/2013-06-11-file-templates-in-intellijidea-do-not-repeat-yourself/03.png %}


