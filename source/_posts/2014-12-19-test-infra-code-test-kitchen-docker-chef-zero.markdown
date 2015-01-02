---
layout: post
title: "Testing Infra Code - Test-Kitchen, Docker, Chef"
date: 2014-12-19 15:18:50 +0530
comments: true
categories: 
---

In one of my recent project we had figured out how important it is to test your infrastructure code.So here in this blog I am going to discuss in detail about the good practices and technologies that we used to do TDD for infra code.
  
In this blog I will be using three techs - "Test-kitchen, Docker and Chef" to do the demo.
This post will cover my talk in <a href="https://rootconf.in/2014/conference#1045-testing-infrastructure-code-using-test-kitchen-doc">#Rootconf - 2014</a>

The workflow and practices can be followed with other equivalent tools as well.

<!-- more -->

###*So here are the topics that we are going to cover in this blog*###

*   Why TDD for infra code ?
*   What is test-kitchen ?
*   Provision instances on demand => kitchen-docker
*   Configure the instances => Chef-zero
*   How to test the infra code => Test-kitchen
*   A Quick Demo

###*What are we trying to solve here*###
 
Lets discuss about the four problems and their solutions above that we see quite often in every project.

**1. Manual steps to setup server:** Every server having hardcoded configurations e.g: Data directory , user config , etc.The result, well , setting up takes hours even though you are following the same steps everytime.In short every server becomes a "work of art".As a result it takes months to finish your deployment.

**Solution:** Model server configurations, parameterize everything, relationships with other servers, packages in code.This is called **_Configuration Management_**.In our demo we are doing to use Chef as the Configuration Management tool to explain how it is done.

**2. No track of what changes were made when:** Another very common issue that we see often is loosing track of what changed.Ever have been in a situation when you come to work one morning and your app server stops working , and you have no idea what changed or who changed.And then you spend one full day to finally find that the iptables rules have changed .

**Solution:** We can solve the above issue if we follow step 1 and verion control that code . We will discuss more about **_Infra As Code_** in the blog.

**3. Code works in local host , not in Production:** A classic developer quote ;) Ever wonder why is that so.Well the answer is pretty simple , inconsistency between dev, QA and Prod environment.If we want the same code on a developer laptop to go and build an app that will be deployed in production, then why dont we have same environment in dev as in production.

**Solution:** Host a production like environment right at your dev laptop and the same in QA.So all devs and QAs have a prod like environment to test the same code that will eventually go in production.This way more bugs are caught before it goes to production.We will see how we can achieve that using **_Test-kitchen_** more coming forward.

**4. Testing takes forever:** Well last but not the least , testing the infra code takes longer than ever.Reason can be not having environment to test the Infra-code.

**Solution:** Scalable **_Environment On Demand_**. Have an environment that can be brought up , tested with the infra code and destroyed and recreated on demand in a timeline of 5 minutes - Now how does that sound ;)

Well all of the above and more can be done that will cut down you release cycle from months to weeks to days to minutes.In our project where we had followed all of the above our release cycle was cut down from twice a week to mutiple time in a day and the deployment time was brought down from 2 hours to 10 minutes.So let us discuss how this can be done in a real world.

###Infrastructure As Code###

* Treat your infrastructure as Code
* Server configuration, packages installed, relationships with other servers etc should be modeled with code to be automated,removing manual steps prone to errors.
* Use version controlling for that code.
* For instance we can (and should!)
  * Tag, branch and release the code that define our servers.
  * Have a lifecycle that covers different stages through the infrastructure code, ie. dev, QA, production.
  * Continuously test our infrastructure as we make changes.

