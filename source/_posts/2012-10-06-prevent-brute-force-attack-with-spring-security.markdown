---
layout: post
title: "Prevent brute force attack with Spring Security"
date: 2012-10-06 15:58
comments: true
alias: /2012/10/prevent-brut-force-attack-with-spring.html
categories: [Groovy, Grails, Security, Spring]
---
Spring Security can do lot of stuff for you. Account blocking, password salt. But what about brute force blocker. That what you have to do by yourself. Fortunately spring is quite flexible framework so it is not a big deal to configure it.

Let me show you little guide how to do this for Grails application.
<!--more-->
First you have to enable springSecurityEventListener in your Config.groovy
grails.plugins.springsecurity.useSecurityEventListener = true

then implement listeners
in /src/bruteforce create classes
{% codeblock lang:groovy %}

/**
Registers all failed attempts to login. Main purpose to count attempts for particular account ant block user

*/
class AuthenticationFailureListener implements ApplicationListener {

    LoginAttemptCacheService loginAttemptCacheService

    @Override
    void onApplicationEvent(AuthenticationFailureBadCredentialsEvent e) {
        loginAttemptCacheService.failLogin(e.authentication.name)
    }
}
{% endcodeblock %}
next we have to create listener for successful logins 
in same package
{% codeblock lang:groovy %}

/**
 Listener for successfull logins. Used for reseting number on unsuccessfull logins for specific account
*/
class AuthenticationSuccessEventListener implements ApplicationListener{

    LoginAttemptCacheService loginAttemptCacheService

    @Override
    void onApplicationEvent(AuthenticationSuccessEvent e) {
        loginAttemptCacheService.loginSuccess(e.authentication.name)
    }
}
{% endcodeblock %}
We were not putting them in our grails-app folder so we need to register these classes as spring beans.
Add next lines into grails-app/conf/spring/resources.groovy
{% codeblock lang:groovy %}

beans = {
    authenticationFailureListener(AuthenticationFailureListener) {
        loginAttemptCacheService = ref('loginAttemptCacheService')
    }

    authenticationSuccessEventListener(AuthenticationSuccessEventListener) {
        loginAttemptCacheService = ref('loginAttemptCacheService')
    }
}
{% endcodeblock %}
You've probably notice usage of LoginAttemptCacheService loginAttemptCacheService
Let's implement it. This would be typical grails service 
{% codeblock lang:groovy %}

package com.grygoriy

import com.google.common.cache.CacheBuilder
import com.google.common.cache.CacheLoader
import com.google.common.cache.LoadingCache

import java.util.concurrent.TimeUnit
import org.apache.commons.lang.math.NumberUtils
import javax.annotation.PostConstruct

class LoginAttemptCacheService {

    private LoadingCache attempts;
    private int allowedNumberOfAttempts
    def grailsApplication

    @PostConstruct
    void init() {
        allowedNumberOfAttempts = grailsApplication.config.brutforce.loginAttempts.allowedNumberOfAttempts
        int time = grailsApplication.config.brutforce.loginAttempts.time

        log.info "account block configured for $time minutes"
        attempts = CacheBuilder.newBuilder()
                   .expireAfterWrite(time, TimeUnit.MINUTES)
                   .build({0} as CacheLoader);
    }

    /**
     * Triggers on each unsuccessful login attempt and increases number of attempts in local accumulator
     * @param login - username which is trying to login
     * @return
     */
    def failLogin(String login) {
        def numberOfAttempts = attempts.get(login)
        log.debug "fail login $login previous number for attempts $numberOfAttempts"
        numberOfAttempts++

        if (numberOfAttempts &gt; allowedNumberOfAttempts) {
            blockUser(login)
            attempts.invalidate(login)
        } else {
            attempts.put(login, numberOfAttempts)
        }
    }

    /**
     * Triggers on each successful login attempt and resets number of attempts in local accumulator
     * @param login - username which is login
     */
    def loginSuccess(String login) {
        log.debug "successfull login for $login"
        attempts.invalidate(login)
    }

    /**
     * Disable user account so it would not able to login
     * @param login - username that has to be disabled
     */
    private void blockUser(String login) {
        log.debug "blocking user: $login"
        def user = User.findByUsername(login)
        if (user) {
            user.accountLocked = true;
            user.save(flush: true)
        }
    }
}
{% endcodeblock %}
We will be using CacheBuilder from google guava library. So please add next lines to BuildConfig.groovy
{% codeblock lang:groovy %}

    dependencies {
        runtime 'com.google.guava:guava:11.0.1'
    }
{% endcodeblock %}
And the last step we will add service configuration to Config.groovy
{% codeblock lang:groovy %}

brutforce {
    loginAttempts {
        time = 5
        allowedNumberOfAttempts = 3
    }
{% endcodeblock %}
That is it, you ready to run you application.
For typical java project almost everything will be the same. Same listeners and same services.

More about Spring Security Events
More about caching with Google guava

Grails users can simple use this plugin https://github.com/grygoriy/bruteforcedefender
UPD: Plugin now at http://grails.org/plugin/bruteforce-defender
Enjoy :)
