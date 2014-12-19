---
layout: post
title: "Environment On Demand"
date: 2014-12-18 19:34:23 +0530
comments: true
categories: 
---
  We all know how important it is to have automated environment in your project. 
  In our regular work we speak about automated build and deployment in enterprises, very less is spoken about or given important to the initial setup which is - ***Provisioning the virtual environments*** 

  Automated provisioning of environment on demand is equally important as is automated build and deployment. 

  It is the entire workflow :- 

  ***Automated Environment on Demand => Config Management => Automated Build and Deployment (Continuous Integration)***

  Earlier this year in March, as part of an in house initiative, we started building a POC of Environment on Demand using Openstack .The technical stack we used are :

  ***Hardware:***

  5 machines - Each with 8 GB ram and 250 GB disk space

  And the setup goes as below:

    1.Windows Server 2008 R2 - Vcenter Server
    2.Vmware Exsi 5.5 - VMware Hosts (2)
    3.Centos 6.5 - KVM Server
    4.Centos 6.5 + Openstack RDO (Redhat Openstack) - Openstack server

  Using the above infrastructure we had setup an openstack-automated environment with VMware and KVM as the hypervisors. Below diagram is an overview of the setup.


  ![screenshot1](/source/images/Slide1.jpg)


####*Some Openstack Details:*####
--------------------------------
  We used Redhat openstack (RDO) for the openstack setup. The reason behind using RDO is that it serves our purpose of supporting most enterprises and found to be stable and easier to configure.
  
  **For compute:** we used Openstack Nova to setup instances on multiple hypervisors. Our list of hypervisors included:

  ***1.KVM***
  
  ***2.Vmware***

  **For networking:** we used Openstack Neutron as the network engine that manages the network creation in openstack with the help of various supported plugins. In this particular demo we used Neutron-Open-Vswitch-Plugin for creating a flat dhcp network. 
  We will be working further on using other plugins that supports physical switch(Cisco/Juniper/Extreme) for managing VLANs and GRE tunnels.

  **For Imaging:** we used Openstack Glance as the image service for building images for kvm (qcow) and VMware (vmdk). Currently it supports Centos and Ubuntu images for both kvm and VMware.

  This video will give a quick walk-through of the above setup in details. 

<ul>
<li>
<iframe width="420" height="315" src="//www.youtube.com/embed/TlHS8e44BEo" frameborder="0" allowfullscreen></iframe>>
</li>
</ul>

There is a lot refactoring that has to go in it, but we are just starting :)

Watch out this space for more updates.

<!-- more -->