# Project_1_Azure_Cloud-Setup
Project 1 submission Elk Stack Azure set up 

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Project_1_Azure_Cloud-Setup/Images/Full _Cloud_Diagram_RRS.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

(Resources\Ansible\Filebeat-playbook.yml)


  name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-$

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Available, in addition to restricting External access to the network.
-  What aspect of security do load balancers protect? What is the advantage of a jump box?

Load balancers add additionl layer of security to the webservers by routing traffic to so that no one server can become overwhelmed with traffic 
helping to avoid DDoS attacks.  

The advantage of a jump box is it allows you access to the internal network with out having a machine facing the outside leaving it vulnerable for attack.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system health.
- Metricbeat collects Operating system information such as uptime and services running on the server
-Filebeat collects data on log files for security purposes

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Security Group | Virtual Network |  Operating System  |
|----------|----------|------------|----------------|-----------------|--------------------|
| Jump Box | Gateway  | 10.0.0.4   |   RedTeam NSG  |  RedTeam Vnet   |       Linux        |
| Web-01   |Web Server| 10.0.0.5   |   RedTeam NSG  |  RedTeam Vnet   |       Linux        |
| Web-02   |Web Server| 10.0.0.6   |   RedTeam NSG  |  RedTeam Vnet   |       Linux        |  
| Web-03   |Web Server| 10.0.0.7   |   RedTeam NSG  |  RedTeam Vnet   |       Linux        |
| ELK	     |Log Server| 10.1.0.4   |   RedTeam NSG  |  RedTeam Vnet   |       Linux        | 

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 65.128.50.54

Machines within the network can only be accessed by SSH.
- Which machine did you allow to access your ELK VM? What was its IP address?
The Jump Box has access to the ELK VM it's IP is 10.0.0.4

A summary of the access policies in place can be found in the table below.

|    Name     | Publicly Accessible | Allowed IP Addresses  |
|-------------|---------------------|-----------------------|
|  Jump Box   | Yes                 |Personal Workstation IP|
|Load Balancer| Yes                 |Personal Workstation IP|
|   Web-01    | No                  | 10.0.0.4              |
|   Web-02    | No                  | 10.0.0.4              |
|   Web-03    | No                  | 10.0.0.4              |
| ELK Server  | Yes                 |Personal Workstation IP|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-Automating the process allows for consitency and saves time by streamlining and allowuing you to rollout multiple servers faster.

The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installs docker.io
- Installs PIP3
- Installs Docker Pythin Modules
- Increases Memory
- Downloads and launches docker conatiner

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images\Screenshots\sudo_docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1  10.0.0.5
- Web-2  10.0.0.6
- Web-3  10.0.0.7

We have installed the following Beats on these machines:
- We installed Filebeat and Microbeat on the Elk server for logging.

These Beats allow us to collect the following information from each machine:
-  Microbeat collect metrics from your host systems and services. CPU to memory and other sytem and services statistics.   filebeat on the otherhand collects and centralizezes system logs and files
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-ELK.yml file to /etc/ansible/.
- Update the hosts file with 'nano /etc/ansible/hosts'  file to include [ELK] then include the server IP
- Run 'ansible-playbook /etc/ansible/install-Elk.yml' from the jump-box ansible container, and navigate to ELK server by SSH and run 'sudo docker ps' to verify that ELK server is running.   You should then beable to access the ELK server by URL http://52.165.174.249:5601/app/kibana.

Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine?  How do I specify which machine to install the ELK server on versus which to install Filebeat on? you will update the hosts file to include [Elk] and include the selserver IP, in this case 10.1.0.4.   at this point you will then configure your install-ELK.yml file to install on hosts:ELK
- _Which URL do you navigate to in order to check that the ELK server is running? Public IP add of your Elk server -  http://52.165.174.249:5601/app/kibana#/home

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

We did not download the playbook we created a new document  nano /etc/ansible/install-ELK.yml
nano /etc/ansible/hosts
add:
[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3
ansible-playbook /etc/ansible/install-Elk.yml
