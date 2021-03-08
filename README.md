# Monash_Work
Repo for work from Monash
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Diagrams/Network_Diagram_ELK.png](https://github.com/RedLeader21/Monash_Work/blob/main/Diagrams/Network_Diagram_ELK.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

The ansible-playbooks install-elk and filebeat-playbook are needed to create and implement the Elk-Server

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Avalible, in addition to restricting access to the network.

Load balancers protect the avalibility aspect of the CIA triad by ensuring that network traffic is appropriately distributed accoss avalible servers. 

The advantage of using a jump box is that it is a highly secured machine which is used for Admin tasks only. It is never used for high risk activities such as emails or internet access, and as such is at a much lower risk of being effected by human error

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
Filebeat collects log records
Metricbeat records usage statistical data from the operating system and from services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Webserver| 10.0.0.5   | Linux            |
| Web-2    | Webserver| 10.0.0.6   | Linux            |
| Web-3    | Webserver| 10.0.0.9   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
110.144.9.109

Machines within the network can only be accessed by SSH from the ansible container on the jumpbox vm(internal ip 10.0.0.4).
The only manchine that can access the ELK machine is the Jumpbox(internal ip 10.0.0.4)
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 110.144.9.109        |
|webservers| No                  | 10.0.0.4             |
|   ELK    | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
This is advantageous because it means that all machines will be set up the same every time, regardless of the amount of machines, their physical locations, or the skill level of the admin

The playbook implements the following tasks:
- Install Docker and Python3
- Permanently increase virtual memory
- Download and launch ELK container
- Enable the ELK container to startup aitomatically on booting the macine

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[Ansible/ELK_Cap.png](https://github.com/RedLeader21/Monash_Work/blob/main/Ansible/ELK_Cap.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5
10.0.0.6
10.0.0.9


We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat: Filebeat is a lightweight piecec of software for parsing log data to the ELK server. Filebeat collects log events and forwards them to the ELK stack for indexing.
Eg: Feb 27, 2021 @ 22:44:58.000	agent.hostname:Web-1 agent.id:02ad5506-671f-4e53-951a-3ed939a4d913 agent.type:filebeat agent.ephemeral_id:c6493d9d-56ec-4bc2-a031-e027acc3bfa8 agent.version:7.4.0 process.name:sshd process.pid:1215 log.file.path:/var/log/auth.log log.offset:73,537 fileset.name:auth message:Received signal 15; terminating. input.type:log @timestamp:Feb 27, 2021 @ 22:44:58.000 ecs.version:1.1.0 service.type:system host.hostname:Web-1 host.name:Web-1 event.timezone:+00:00 event.module:system event.dataset:system.auth _id:J6j_8XcBwYk-RgF98qLu _type:_doc _index:filebeat-7.4.0-2021.03.02-000001 _score: -
This indicates that there was an attempt to connect via ssh

Metricbeat: Metricbeat collects useage metrics from the OS and from services running on the server. 
Mar 6, 2021 @ 10:05:26.915	@timestamp:Mar 6, 2021 @ 10:05:26.915 agent.type:metricbeat agent.ephemeral_id:c42c7df5-a596-41e8-ad2a-5c6aa957576e agent.hostname:Web-1 agent.id:2ffb9935-ec0c-4030-82b2-fd68488d0b22 agent.version:7.4.0 cloud.instance.id:93ee569c-dc52-4e5f-9860-d83a317876e9 cloud.instance.name:Web-1 cloud.machine.type:Standard_B1s cloud.region:westus2 cloud.provider:az metricset.name:cpu metricset.period:10,000 event.dataset:docker.cpu event.module:docker event.duration:1655.4 service.address:/var/run/docker.sock service.type:docker docker.cpu.kernel.ticks:28,300,000,000 docker.cpu.kernel.pct:0%
This particular example indicates the load that the vCPU of web-1 was under at the time 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat.yml file to /etc/ansible/roles/files/.
- Update the configuration file to include the Private IP of the Elk-Server under the ElasticSearch and Kibana sections
- Run the playbook, and navigate to the Elk web site to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ filebeat-playbook.yml is to be copied to /etc/ansible/roles/files/
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ Host.cfg bust be updated with the private IP(s) that are to recieve the playbook along with an alias for the groups of IP(s). When configuring the playbook the alias is used to designate which servers are to recieve that playbook
- _Which URL do you navigate to in order to check that the ELK server is running? [Public_IP_Of_ELK_Server]:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- ssh sysadmin@(JumpBox_Public_IP)
- sudo docker container list -a
- sudo docker start (Name_Of_The_Container)
- sudo docker attach (Name_Of_The_Container)
- cd /etc/ansible/
- ansible-playbook elk.yml
- cd /etc/ansible/roles/
- ansible-playbook filebeat-playbook.yml
- [Public_IP_Of_ELK_Server]:5601/app/kibana to test
