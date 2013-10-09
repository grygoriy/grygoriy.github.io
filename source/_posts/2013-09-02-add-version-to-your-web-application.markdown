---
layout: post
title: "Add version to your web application"
date: 2013-09-02 18:12
comments: true
categories: [Java, Maven, Web]
---
For many times I was facing issue when developers or QC could not figure out which application was deployed to test environment. There were attempts to guess checking functionality. Also this cause lot of misunderstanding inside of team. Here I will add version for Java web application build by Maven. So you could always check this version with specific URL.

Before describing solution for Maven I would like to mention very useful plugin for Grails in case you are using it. Build-info [http://grails.org/plugin/build-info](http://grails.org/plugin/build-info). This plugin will do everything you want end even more.
<!--more-->
Ok, so we will start with maven web project. Assuming you have one.
We need to put file called _version.txt_ into _src/main/webapp/_. You can pick up any file name you want, this name will be used to access file with URL. Content of my file is:

{% codeblock %}
Project version - ${project.version}
Last commit hash - ${buildNumber}
Build at - ${buildDate}
{% endcodeblock %}
This is template which we will be filtering with maven. So here is maven config.
{% codeblock lang:xml %}
<project>
    <!-- Configuration for Git VCS to grab commit hash -->
    <scm>
        <connection>scm:git:file://.</connection>
        <developerconnection>scm:git:file://.</developerconnection>
        <url>scm:git:file://.</url>
    </scm>
    <build>
      <plugins>
            <!-- Plugin is used to generate build time -->
            <plugin>
                <groupid>org.codehaus.mojo</groupid>
                <artifactid>buildnumber-maven-plugin</artifactid>
                <version>1.1</version>
                <executions>
                    <execution>
                        <phase>generate-resources</phase>
                        <goals>
                            <goal>create</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <timestampformat>{0, date, yyyy-MM-dd HH:mm:ss}</timestampformat>
                </configuration>
            </plugin>
            <!-- We need to include version.txt file into filter list. So maven will filter it when will be building *.war -->
            <plugin>
                <groupid>org.apache.maven.plugins</groupid>
                <artifactid>maven-war-plugin</artifactid>
                <version>2.3</version>
                <configuration>
                    <webresources>
                        <resource>
                            <filtering>true</filtering>
                            <directory>src/main/webapp</directory>
                            <includes>
                                <include>version.txt</include>
                            </includes>
                        </resource>
                    </webresources>
                </configuration>
            </plugin>

      </plugins>
    </build>
    <properties>
        <!-- Properties to format date output -->
        <maven .build.timestamp.format="">dd-MM-yy_HH:mm</maven>
        <builddate>${maven.build.timestamp}</builddate>
    </properties>
</project>
{% endcodeblock %}
This is it. Now you can build your application 
{% codeblock %}
mvn package
{% endcodeblock %}
Deploy and check under url [http://localhost:8080/version.txt](http://localhost:8080/version.txt)

Output should look like this
{% img /images/2013-09-02-add-version-to-your-web-application/version_view.png %}
Enjoy.

