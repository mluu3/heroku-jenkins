# Jenkins on Heroku

https://jenkins-minh.herokuapp.com/job/seleniu-webdriver-cucumber/

Currently this is just a test setup. Because the dyno is cycled every 24 hours 
the jenkins setup has to be re-configured every 24 hours as well .

Warning:

* login by user: admin and password: 721673bf483a4f63ac5e26beba7c8484
* job: https://jenkins-minh.herokuapp.com/job/seleniu-webdriver-cucumber/
* Please just view config for checking solution, DON'T TRIGGER job because it is deployed on free domain with limit memory. It will be terminated immediately.

##Build

macOS
```
/usr/local/opt/openjdk@11/bin/java -Dmail.smtp.starttls.enable=true -jar jenkins-1.638.war --httpListenAddress=127.0.0.1 --httpPort=8080
```

Note: please make sure .jenkins doesn't exist in your computer. Or using .jenkins in heroku-jenkins project to override your .jenkin folder.

#How-to:
https://localhost:8080
##Configuration in jenkins:

* Manage jenkin -> manage plugins -> Install Maven Integration plugin and Publish HTML report
* Manage jenkin -> Global Tool Configuration -> add Maven and JDK
* New item -> maven project (set Title: cucumber-test)

##Configuration in job:

* Dashboard -> cucumber-test -> configure
  * General: GitHub project https://github.com/mluu3/selenium-webdriver-cucumber.git/
  * Source Code Management: git -> repository URL: https://github.com/mluu3/selenium-webdriver-cucumber.git ,add Credentials (Private) and branch: */master
  * Build Trigger: check on Build whenever a SNAPSHOT dependency is built
  * Build Environment: check on Delete workspace before build starts
  * Build: root POM: pom.xml , goals and options: clean test allure:report -DisHeadless=true
  * Post-build actions: add Publish HTML report -> HTML directory to archive: target/allure-report
