---
layout: post
title: "Test Infra Code - Test-Kitchen, Docker, Chef"
date: 2014-12-19 15:18:50 +0530
comments: true
categories: 
---

In one of my recent project we had figured out how important it is to test your infrastructure code.So here in this blog I am going to discuss in detail about the good practices and technologies that we used to do TDD for infra code.This post will cover my talk in 
<ul>
  <li>
  <a href="#Rootconf - 2014">https://rootconf.in/2014/conference#1045-testing-infrastructure-code-using-test-kitchen-doc</a>
</li>
</ul>  


I this blog I will be using three techs - "Test-kitchen, Docker and Chef" to do the demo.

The workflow and practices can be followed with other equivalent tools as well.

<!-- more -->

###Topics to be covered:###
===========================

Why TDD for infra code
What is test-kitchen
Provision instances on demand => kitchen-docker
Configure the instances => Chef-zero
Test the infra code => Test-kitchen
Quick Demo

###What are we trying to solve:###
==================================

Problems:
Manual setups per server. Each Server becomes a "work of art".
No track of what changes were made when.
Code works in local host , not in production.
Testing takes forever

Solutions:
Model server configs, relationships with other servers, packages in code.(Configuration Management)
Infra as Code, Code to be version controlled.
Host production like environment locally.
Scalable Environment On Demand .

###Infrastructure As Code:###
=============================

Treat your infrastructure as Code
Server configuration, packages installed, relationships with other servers etc should be modeled with code to be automated, removing manual steps prone to errors.
Use version controlling for that code.
For instance we can (and should!)
tag, branch and release the code that define our servers.
have a lifecycle that covers different stages through the infrastructure code, ie. dev, QA, production.
continuously test our infrastructure as we make changes.


