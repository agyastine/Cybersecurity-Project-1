# Cybersecurity-Project-1
Project 1
Automated ELK Stack Deployment
This document contains the following details:
Description of the Topology
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Access Policies
Description of the Topology
This repository includes code defining the infrastructure below.

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA (D*mn Vulnerable Web Application). This is to help better understand the processes of securing web applications.
Load balancing ensures that the application will be reliable and highly available, in addition to restricting inbound access to the network. The load balancer ensures that it distributes incoming traffic to the network by efficiently distributing across multiple servers. It gives control access to only authorized users to connect. It helps in scaling up and down servers based on demands on the servers.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network, as well as watch system metrics, such as CPU usage; attempted SSH logins; sudo escalation failures; etc.
The following ansible-playbooks are needed to create and install DVWA and the ELK-server
my-playbook.yml - for installing DVWA servers
elk-playbook.yml - for installing ELK-SERVER
filebeat-playbook.yml - for installing and configuring Filebeat on ELK-SERVER and Web-1, Web-2 and DVWA servers
metricbeat-playbook.yml - For installing and configuring Metricbeat on ELK-SERVER and Web-1, Web-2 and DVWA servers
The configuration details of each machine may be found below.
Name
Function
IP Address
Operating System
Jump-Box-Provision
Gateway - Ansible - Docker
10.0.0.4
Linux
Web-1
Web Server
10.0.0.5
Linux
Web-2
Web Server
10.0.0.6
Linux
DVWA-VM2
Web Server
10.0.0.7
Linux
ELK-SERVER
Monitoring - ELK Stack
10.2.0.4
Linux

In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box. The load balancer's targets are organized into the following availability zones:
Availability Zone 1: Web-1 + Web-2 + DVMA-VM2
Availability Zone 2: ELK-SERVER
ELK Server Configuration
The ELK-SERVER VM exposes an Elastic Stack instance. Docker is used to download and manage an ELK container.
Rather than configure ELK manually, we opted to develop a reusable Ansible Playbook to accomplish the task. This playbook is duplicated below.
To use this playbook, one must log into the Jump-Box-Provision, then issue: ansible-playbook install_elk.yml elk. This runs the install_elk.yml playbook on the elk host.
Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the jump-box-provision machine can accept connections from the Internet. Access to this machine is only allowed from the IP address: (home network IP)
Machines within the network can only be accessed by each other. The Web-1, Web-2 and DVMA-VM2, VMs send traffic to the ELK-SERVER.
A summary of the access policies in place can be found in the table below.

Name
Publicly Accessible
Allowed IP Addresses
Jump-Box-Provision
Yes
52.249.248.65
Web-1
No
10.0.0.1-254
Web-2
No
10.0.0.1-254
DVMA-VM2
No
10.0.0.1-254
ELK-SERVER
No
10.2.0.1-254

Elk Configuration
Ansible was used to automate configuration of the ELK-SERVER machine. No configuration was performed manually, which is advantageous because it increases accuracy, and saves time by eliminating human error in reissuing commands.
The main advantage of automating the installation process is to deploy multiple servers easily and quickly without having to physically setup each server.
The playbook implements the following tasks:
Install Docker.oi
Install pip3
Install Docker python module
Increase virtual machine memory
Download and Launch docker elk container
The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

The ELK playbook is duplicated below.
---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: sysadmin
  become: true
  tasks:
 
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Docker module
      pip:
        name: docker
        state: present

    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

- name: Enable service docker on boot
  systemd:
     name: docker
     enable: yes

Note: Allow remote_user = <username> in /etc/ansible/ansible-cfg file 
Target Machines & Beats
This ELK-SERVER is configured to monitor the Web-1, Web-2 and DVMA-VM2, VMs, at 10.0.0.5, 10.0.0.6 and 10.0.0.7,  respectively.
Installed the following Beats on these machines: Filebeat and Metricbeat
The filebeat playbook implements the following tasks
Filebeat to;
Download filebeat .deb file
Install filebeat
Drop in filebeat.yml
Enable and Configure System Module
Setup filebeat
Start filebeat service
The metricbeat playbook implements the following tasks
Metricbeat to;
Download metricbeat .deb file
Install metricbeat
Drop in metricbeat.yml
Enable and Configure docker module for metricbeat
Setup metricbeat
Start metricbeat service
These Beats allow us to collect the following information from each machine:
Filebeat: Filebeat watches for log files and collects log events that is Lightweight Log Analysis and Elasticsearch. It detects changes to the filesystem. Specifically, we use it to collect Apache logs.
Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.
The playbook below installs Metricbeat on the target hosts. The playbook for installing Filebeat is not included, but looks essentially identical — simply replace metricbeat with filebeat, and it will work as expected.
---
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
    # Use command module
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

    # Use command module
  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

    # Use copy module
  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

    # Use command module
  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

    # Use command module
  - name: setup metric beat
    command: metricbeat setup

    # Use command module
  - name: start metric beat
    command: service metricbeat start
Using the Playbooks
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
Update the configuration files to include the Private IP (10.2.004) of the ELK-Server to the ElasticSearch and Kibana Sections of the Configuration File
Run the playbook, and navigate to http://PublicIP(ELK-SERVER):5601/app/kibana to check that the installation worked as expected.
The playbook files are:
elk-playbook.yml - used for installing ELK Server
filebeat-playbook.yml - Used for installing and configuring Filebeat on ELK-SERVER and Web-1, Web-2 and DVMA-VM2 servers
metricbeat-playbook.yml - Used to install and configure Metricbeat on Elk Server and Web-1, Web-2 and DVMA-VM2 servers
The playbooks are copied in the;  /etc/ansible/
Updates the /etc/ansible/hosts file to run the playbook on a specific machine
How do I specify which machine to install the ELK server on versus which to install Filebeat on?
Used /etc/ansible/hosts to specify which machine to install ELK server vs Filebeat and Metricbeat
Used http://PublicIP(ELK-SERVER):5601/app/kibana to check if the ELK server is running
Running the Anisble for the Elk-Server;
ssh sysadmin@<Jump-Box-Provison_PublicIP>
sudo docker container list -a  (Locate the ansible container)
sudo docker start wonderful_mirzakhani
sudo docker attach wonderful_mirzakhani
cd /etc/ansible
ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
cd /etc/ansible/
ansible-playbook beats-playbook.yml (Installs and Configures Beats)
Opened a new browser on Personal Workstation, navigated to (http://PublicIP(ELK-SERVER):5601/app/kibana) - to bring up Kibana Web Portal

Then, run: curl http://10.0.0.8:5601. This is the address of Kibana. If the installation succeeded, this command should print HTML to the console.

References
Filebeat: Lightweight Log Analysis & Elasticsearch. (n.d.). Retrieved March 01, 2021, from https://www.elastic.co/beats/filebeat 
Metricbeat: Lightweight Shipper for Metrics. (n.d.). Retrieved March 01, 2021, from https://www.elastic.co/beats/metricbeat

