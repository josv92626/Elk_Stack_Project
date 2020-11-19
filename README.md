# Elk_Stack_Project

## These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above, or select portions of the provided files may be used to install specific pieces of the deployment - such as Filebeat.

## This document contains the following details:

### Description of the Topology
### Access Policies
### ELK Configuration
### Beats in Use
### Machines Being Monitored
### How to Use the Ansible Build

# Description of the Topology

#### The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application. The usage of which is to test various regular web vulnerability, with various difficultly levels, in a simple UI.

#### First and foremost - the load balancer's off-loading function, or distribution of traffic across multiple servers, mitigates Denial of Service attacks. This highly valuable security measure alone highlights the benefit of a load balancer.
#### Load balancing ensures that the application will be highly available, scalable, and reliable - in addition to restricting unsecured access to the network. Load balancers also offer a health probe function, obtaining reports of servers with issues and stops sending traffic to said servers.

#### Having multiple virtual machines host the DVWA establishes a wider penetration testing environment, and if one becomes unavailable the others will remain active and the functions of the load balancer further come into play.

#### The virtual network and allocated security group the DVWA machines have been configured within ensures the filtering of network traffic via security rules, such as which ports are allowed access. This is true for the virtual network and allocated security group for the ELK server as well.

##### Note that the networks are peering - this allows both networks to seemlessly connect with each other, and simplifies configurations of the resource group containing the entire infrastructure.

##### Filebeat collects data about the file system, ones that you can specify as may be necessary with a larger organization, and forwards them to Elasticsearch and/or Logstash for indexing.

#### Metricbeat (an accompanying service to Filebeat) collects server metrics - such as uptime, hardware utilization, peak response time, etc. This is particularly important tool for monitoring services such as Apache2 or MySQL. This data is also forwarded to Elasticsearch and/or Logstash.

##### Note that Filebeat is a separate entity within the diagram, but it is a running service on the DVWA host machines.

## Access Policies

#### The machines on the internal network are not exposed to the public Internet.

#### Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from machines that have the SSH key pair used while configuring the server (public key loaded onto the VM, SSH using the id_rsa file containing the private key).

#### Machines within the network can only be accessed via SSH port 22 through the Docker container loaded on the Ansible machine.

##### The ELK server can only be accessed via port 22 (SSH) from the Docker container, which has the key pair, and via HTTP on port 5601 - the latter being a security rule that needs to be established in the configuration process.

##### Additionally, placing the daemon.json file inside the /etc/docker directory on the Jump-Box-Provisioner will ensure that the container will be within the same subnet of the virtual network.

## Elk Configuration
#### Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the process eliminates potential human error and makes the process significantly more efficient - particularly while utilizing a bash script to establish multiple configurations with one command. There are, however, specific elements that will need to be modified to accurately configure according to the parameters of the topology (virtual networks and their respective IP addresses and/or subnets, as was modified in this particular case for the filebeat-config.yml).

#### The playbook ansible-config.yml is used to configure Ansible on the host machine.

#### The playbook Elk-Install.yml run using the 'ansible-playbook ' command is the necessary action to establish ELK.

##### Note that within the playbook, there is a section establishing the memory for the container. Without expanding the memory, the container cannot run and thus the application will not launch.

#### The playbook implements the following tasks:
##### Installs necessary files for ELK to run as intended.
##### Configures the hosting server and the ELK apps themselves.
##### Lauches the Docker container in which the ELK Stack has been configured.

## Target Machines & Beats
#### This ELK server is configured to monitor the Red-Team-Web virtual machines hosting the DVWA, #1 and #2.

#### We have installed the following Beats on these machines:

##### Filebeat.
##### Metricbeat.
##### Packbeat.
#### The funtions of Filebeat and Metricbeat were covered at the beginning of this readme. Packbeat collects packets that pass through the network card, generating a trace of the network activity and forwarding this data to the ELK stack for analysis.

## Using the Playbook
#### In order to use the playbook, you will need to have an Ansible control node already configured. The Jump-Box-Provisioner serves this purpose.

#### SSH into the control node and follow the steps below:

#### Update the hosts file within the Ansible directory (/etc/Ansible/) to include the webservers (Red Team VMs) and the ELK server. Referenced in this screenshot. While in this directory, run the command 'mkdir files' - this is crucial for the third step.

#### Copy the filebeat-config.yml to the proper directory specified inside said YAML file.

#### Run the playbook, and navigate to to check that the installation worked as expected. As refrenced in the first step, this will involve making a directory called 'files' inside the Ansible directory in order to use this specific configuration.

#### To verify success, you can run the command 'sudo docker container list -a' to verify that the ELK container was pulled and installed. You can then run 'sudo docker start elk' follow by 'sudo docker ps' to verify that the container launched and runs successfully.

#### By then running curl http://52.252.58.97:5601, if the installation was successful and the container is running, the Kibana web application will load in a web browser.
