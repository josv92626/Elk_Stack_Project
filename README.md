# Elk_Stack_Project

##These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above, or select portions of the provided files may be used to install specific pieces of the deployment - such as Filebeat.

This document contains the following details:

Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application. The usage of which is to test various regular web vulnerability, with various difficultly levels, in a simple UI.

First and foremost - the load balancer's off-loading function, or distribution of traffic across multiple servers, mitigates Denial of Service attacks. This highly valuable security measure alone highlights the benefit of a load balancer.

Load balancing ensures that the application will be highly available, scalable, and reliable - in addition to restricting unsecured access to the network. Load balancers also offer a health probe function, obtaining reports of servers with issues and stops sending traffic to said servers.

Having multiple virtual machines host the DVWA establishes a wider penetration testing environment, and if one becomes unavailable the others will remain active and the functions of the load balancer further come into play.
