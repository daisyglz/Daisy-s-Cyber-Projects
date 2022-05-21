# Daisy-s-Cyber-Projects
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Project 1 Red-Team Network Diagram

![Users/daisygonzalez/Desktop/README/Images/Network Diagram.drawio.png: 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the __Ansible___ file may be used to install only certain pieces of it, such as Filebeat.

Filebeat-playbook.yml

Filebeat-configuration.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Dmn Vulnerable Web Application.

Load balancing ensures that the application will be highly _functional____, in addition to restricting _high-traffic____ to the network.

- What aspect of security do load balancers protect? What is the advantage of a jump box? It helps prevent overloading servers as well as optimizes productivity and maximizes uptime. Load balancers also reroute live traffic from one server to another causing it to eliminate single points of failure from attacks. 

Jump-box are a highly secure computers that are used for non-admin tasks in different security zones.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __network___ and system _logs____.
- What does Filebeat watch for? Monitors the log files or locations that you specify, collects log events and forwards them to Elasticsearch
- What does Metricbeat record? Takes metrics and statistics that it collects and ships the output that you specify for this projector is Elasticsearch. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address                             Operating System |
|----------|----------|----------------------------------------|------------------|
| Jump Box | Gateway  | 10.0.0.1-Private 20.53.235.157 Public  | Linux            |
| Web-1    | Server   | 10.0.0.7 Private                       | Linux            |
| web-2    | Sever    | 10.0.0.8 Private                       | Linux            |
| ELK      | Server   | 10.1.0.4-Private  20.70.67.82          | Linx             |



### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Jump-box___ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

69.230.150.21 (LocalHost IP address)

Machines within the network can only be accessed by __Jump-Box-Provisioner___.
- Which machine did you allow to access your ELK VM? What was its IP address?_Jump-Box IP Private Address : 10.0.0.5

A summary of the access policies in place can be found in the table below.
| Name     | Publicly Accessible | Allowed IP Addresses  |
|----------|---------------------|-----------------------|
| Jump Box | Yes                 | 69.230.150.21         |
| Web-1    | NO                  | 10.1.0.4              |
| web-2    | No                  | 10.1.0.4              |
| ELK      | No                  | 10.1.0.4              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-  What is the main advantage of automating configuration with Ansible?_Ansible lets you model even highly complex IT workflows, it is a primary mechanism for breaking a playbook into multiple files.

The playbook implements the following tasks:
- : In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- First I SSH into the Jump-Box (ssh Azadmin@20.53.235.157)
- Start/Attach to the ansible docker (sudo docker start funny_jones) / (sudo docker attach funny_jones)
- Run command /etc/ansible/roles directory and create the ELK playbook (elk.yml) 
- Run the ELK_playbook.yml in that same directory (ansible-playbook elk.yml) 
- Lastly, I will SSH into the ELK-VM to verify the server is up and running. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

-Users/daisygonzalez/Desktop/README/Images/elk.yml.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring
-Web-1 10.0.0.7
-Web-2 10.0.0.8

We have installed the following Beats on these machines:
- /Users/daisygonzalez/Desktop/README/Images/Filebeat Module Status Screenshot .png 
- /Users/daisygonzalez/Desktop/README/Images/Metricbeat Module Status Screenshot.png


These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.
- Filbert is used to collect log files from specific files on remote machines 
-Examples of Filberts can be files that are generated by Apache, Azure tools, the Nginx web server, and MySQI datebases
-Metricbeats collects machine metrics 
- It is a measurement to tell analysts how healthy it is
- Examples of Metricbeat can be CPU usage/uptime

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
-----Filebeat------
- Copy the __filebeat-config.yml___ file to ___/etc/ansible/files__.
- Update the __filebeat-config.yml___ file to include the ELK private IP in lines 1106 and 1806
- Run the playbook, and navigate to http://20.70.67.82:5601/app/kibana to check that the installation worked as expected.

-----Metricbeat----
- Copy the metricbeat-config.yml file to /etc/ansible/files.
- Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.
- Run the playbook, and navigate to http://20.49.3.56:5601/ (ELK-VM public IP) to check that the installation worked as expected.


- Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? Filbert-playbook.yml and it is copy to /etc/ansible/files
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? The file I update to run the playbook on a specific machine is /etc/ansible/hosts file. I specify two separate groups in the etc/ansible/hosts file. One group will be the web servers which has the IPs of the VMs that I will install Filbert to. The other group is named elk servers which will have the IP of the VM I will install ELK to. 
- _Which URL do you navigate to in order to check that the ELK server is running? http://20.70.67.82:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

-----Filebeat--------

---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

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

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes


--------Metricbeat----------


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

    # Use systemd module
  - name: Enable service metricbeat on boot
    systemd:
      name: metricbeat
      enabled: yes
