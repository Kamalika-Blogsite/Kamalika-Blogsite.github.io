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

####*So here are the topics that we are going to cover in this blog*####

*   Why TDD for infra code ?
*   What is test-kitchen ?
*   Provision instances on demand => kitchen-docker
*   Configure the instances => Chef-zero
*   How to test the infra code => Test-kitchen
*   A Quick Demo

####*What are we trying to solve*####

  ###Problems

  1. Manual setups per server.
     Each Server becomes a "work of art".
  2. No track of what changes were made when.
  3. Code works in local host, 
     not in production.
  4. Testing takes forever.





