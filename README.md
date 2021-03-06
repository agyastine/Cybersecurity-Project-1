## Automated ELK Stack Deployment

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Diagram/Project%20Topology.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the main.yml file may be used to install only certain pieces of it, such as Filebeat.

roles/main.yml

  _The playbook files are:_
- my-playbook.yml - for installing DVWA servers
- elk-playbook.yml - for installing ELK-SERVER
- filebeat-playbook.yml - for installing and configuring Filebeat on ELK-SERVER and Web-1, Web-2 and DVWA servers
- metricbeat-playbook.yml - For installing and configuring Metricbeat on ELK-SERVER and Web-1, Web-2 and DVWA servers
### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA (D*mn Vulnerable Web Application). This is to help better understand the processes of securing web applications.

Load balancing ensures that the application will be reliable and highly available, in addition to restricting inbound access to the network. The load balancer ensures that it distributes incoming traffic to the network by efficiently distributing across multiple servers. It gives control access to only authorized users to connect. It helps in scaling up and down servers based on demands on the servers.

Load Balancers defend against distributed denial-of-service (DDoS) attacks and the advantage of the jump box is to control a safer access to the servers 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _logs_ and system _metrics_.
- _Filebeat watches for log files and collects log events that is Lightweight Log Analysis and Elasticsearch._
- _Metricbeat records the metrics from services running on the server and from the operating system regularly._

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
- Access to this machine is only allowed from the IP address: (home network IP)

Machines within the network can only be accessed by _each other. The Web-1, Web-2 and DVMA-VM2, VMs send traffic to the ELK-SERVER.

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible | Allowed IP Addresses |
|--------------------|---------------------|----------------------|
| Jump-Box-Provision | Yes                 | (homeIP Address)     |
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

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker%20-ps.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _Web-1, Web-2 and DVMA-VM2, VMs, at 10.0.0.5, 10.0.0.6 and 10.0.0.7,  respectively._

We have installed the following Beats on these machines:
- _Filebeat_
- _Metricbeat_

These Beats allow us to collect the following information from each machine:
- _Filebeat: Filebeat watches for log files and collects log events that is Lightweight Log Analysis and Elasticsearch. It detects changes to the filesystem. Specifically, we use it to collect Apache logs._
- _Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
- Update the configuration files to include the Private IP (10.2.004) of the ELK-Server to the ElasticSearch and Kibana Sections of the Configuration File
- Run the playbook, and navigate to http://PublicIP(ELK-SERVER):5601/app/kibana to check that the installation worked as expected.

_Where to update Ansible to run playbook:_
- The playbooks are copied in the;  /etc/ansible/
- Updates the /etc/ansible/hosts file to run the playbook on a specific machine
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
- Use /etc/ansible/hosts to specify which machine to install ELK server vs Filebeat and Metricbeat
- Use http://PublicIP(ELK-SERVER):5601/app/kibana to check if the ELK server is running

## _specific commands to run to download the playbook, update the files, etc._
- ssh sysadmin@<Jump-Box-Provison_PublicIP>
- sudo docker container list -a  (Locate the ansible container)
- sudo docker start wonderful_mirzakhani
- sudo docker attach wonderful_mirzakhani
- cd /etc/ansible
- ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
- cd /etc/ansible/
- ansible-playbook beats-playbook.yml (Installs and Configures Beats)
- Opened a new browser on Personal Workstation, navigated to (http://PublicIP(ELK-SERVER):5601/app/kibana) - to bring up Kibana Web Portal

