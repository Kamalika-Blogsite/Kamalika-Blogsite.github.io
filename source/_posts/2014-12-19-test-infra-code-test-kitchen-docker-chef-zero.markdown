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

###*Infrastructure As Code*###

* Treat your infrastructure as Code
* Server configuration, packages installed, relationships with other servers etc should be modeled with code to be automated,removing manual steps prone to errors.
* Use version controlling for that code.
* For instance we can (and should!)
  * Tag, branch and release the code that define our servers.
  * Have a lifecycle that covers different stages through the infrastructure code, ie. dev, QA, production.
  * Continuously test our infrastructure as we make changes.

  {% img right /images/Slide06.jpg 500 500 %}

As can be seen in the diagram, every server is tagged by some chef roles like, Appserver, DBserver etc, under some chef environment either dev, QA or Prod with environment specific values like url , ports etc.And then we have chef recipes/cookbooks/run_lists that they the actual scripts that will ne running on that server.We used Chef-zero as we wanted to use an exact chef-server environment as we used in production.The workflow holds good for any other configuration management tools like Puppet, Ansible etc.

###*Why Test Driven Development for Infrastructure Code*###

* Code works in localhost as well as in production server.
* Quick and immediate feedback while testing Infra code.
* Faster you test code,faster you deploy in Production.
* Shorter Release Cycle for apps.
 
Some real world scenarious which required change in infra code and testing that so that it does not impact existing setup.In our project we had to do come accross some of the following tasks that we managed through chef recipes and roles.

* Changing web ports => e.g: Change app port to 443 
* Adding new app metrics => e.g: Adding nginx config to monitor app health
* Changing app user => e.g: making the app to run under service user
* Add a new user => e.g: Allow a whitelisted user to access the website
* Add basic authentication => e.g: enable basic authentication through apache/nginx
* Configuring a new machine => e.g: Adding additAny of the above can be done using chef recipes 

Any of the above if not tested properly can cause drastic issues in production.For instance if we change the app port to 443 from 80 and say the intermidiate firewall rules are not updated to allow port 443 the the entire website will be down.These loopholes can be caught only if we test the infra code in dev environment right at the begining.

Let us see how the setup looks like for testing infra code using Chef-Zero, Test-Kitchen and Docker.
{% img right /images/test-kitchen.png 500 500 %}

We used Test-kitchen with the docker plugin to build on demand containers and test our chef recipes on then.Test-kitchen uses kitchen.yml file which manages kitchen configurations and setup.We used customized docker images for centos and used docker registry to store and distribute images.

A sample kitchen.yml file looks as below.
<pre><code>---
driver:
  name: docker
driver_config:
  require_chef_omnibus: 10.18.2

  run_command: /usr/sbin/sshd -D -o UseDNS=no -o UsePAM=yes
  use_sudo: false
  provision_command:
    - chown kitchen.kitchen /tmp/kitchen
    - echo 'http_proxy="http://172.17.42.1:8123"' /etc/yum.conf
  http_proxy: http://172.17.42.1:8123
provisioner:
  name: chef_zero
  client_rb:
    http_proxy: http://172.17.42.1:8123
    no_proxy: 127.0.0.1
platforms:
  - name: dev
    driver_config:
      image: "repo:centos"
      platform: "centos"
suites:
  - name: web
    data_bags_path: databags
    roles_path: kitchen_roles
    run_list:
    - recipe[apache]

  - name: db01
    data_bags_path: databags
    roles_path: kitchen_roles
    run_list:
    - role[db]
    - recipe[mongodb::replicaset]

    attributes:
      databag: "acceptance-test"
</code></pre>

