## Automated ELK Stack Deployment

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Diagram/Project%20Topology.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the main.yml file may be used to install only certain pieces of it, such as Filebeat.

roles/main.yml

  - _TODO: Enter the playbook file._

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA (D*mn Vulnerable Web Application). This is to help better understand the processes of securing web applications.

Load balancing ensures that the application will be reliable and highly available, in addition to restricting inbound access to the network. The load balancer ensures that it distributes incoming traffic to the network by efficiently distributing across multiple servers. It gives control access to only authorized users to connect. It helps in scaling up and down servers based on demands on the servers.

Load Balancers defend against distributed denial-of-service (DDoS) attacks and the advantage of the jump box is to control a safer access to the servers 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _logs_ and system _metrics_.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.

| Name                | Function              | IP Address | Operating System |
|-------------------- |-----------------------|------------|------------------|
| Jump-Box-Provision  | Gateway               | 10.0.0.4   | Linux            |
| Web-1               | Web Server            | 10.0.0.5   | Linux            |
| Web-2               | Web Server            | 10.0.0.6   | Linux            |
| DVMA-VM2            | Web Server            | 10.0.0.7   | Linux            |
| ELK-SERVER          | Monitoring -ELK Stack | 10.2.0.4   | Linux            |

In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box. The load balancer's targets are organized into the following availability zones:

Availability Zone 1: Web-1 + Web-2 + DVMA-VM2
Availability Zone 2: ELK-SERVER

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump-box-provision machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Access to this machine is only allowed from the IP address: (home network IP)_

Machines within the network can only be accessed by _each other. The Web-1, Web-2 and DVMA-VM2, VMs send traffic to the ELK-SERVER.__.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible | Allowed IP Addresses |
|--------------------|---------------------|----------------------|
| Jump-Box-Provision | Yes                 | <homeIP Address>     |
| Web-1              | No                  | 10.0.0.1-254         |     
| Web-2              | No                  | 10.0.0.1-254         |
| ELK-SERVER         | No                  | 10.0.0.1-254         |

### Elk Configuration

Ansible was used to automate configuration of the ELK-SERVER machine. No configuration was performed manually, which is advantageous because it increases accuracy, and saves time by eliminating human error in reissuing commands.

The main advantage of automating the installation process is to deploy multiple servers easily and quickly without having to physically setup each server

The playbook implements the following tasks:
- Install Docker.oi
- Install pip3
- Install Docker python module
- Increase virtual machine memory
- Download and Launch docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._1
