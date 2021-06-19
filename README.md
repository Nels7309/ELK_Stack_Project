# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELK_Net_Diagram](../Diagrams/ELK_Net_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat. Below is an example of the filebeat_playbook.yml used to install filebeat.

![filebeat_playbook](../Ansible/filebeat_playbook.yml)

This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.

Load Balancers help with protection a couple different ways. One way is that they help distribute the load of traffic so one or more servers are not overloaded. Another way is that it helps is that if one of the servers goes down for either updating or an attack, the load balancer will transfer traffic to another server preventing outages.

Some of the main advantages of using a Jumpbox is that it allows an administrator to access and change/add security rules to a private network from the public internet. Typically Jumpboxs are configured to only allow certain ip addresses to access them to maintain the network security.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log systems and system resources.

- Filebeat looks for and reports changes made to log systems, some of which can indicate an attack vs normal activity.
- Metricbeat helps monitor the systems reporting the metrics of the system.

The configuration details of each machine may be found below:

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| Jump Box | Gateway   | 10.0.0.5   | Linux            |
| Web-1    | Web server| 10.0.0.6   | Linux            |
| Web-2    | Web server| 10.0.0.7   | Linux            |
| Web-3    | Web server| 10.0.0.9   | Linux            |
| ELK      | Monitoring| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jumpbox machine can accept connections from the internet. Access to this machine is only allowed from my home machine with the sample IP address of 72.50.xxx.xx

Machines within the network can only be accessed by each other. Web-1, Web-2, and Web-3 VM's are allowed to send traffic to the Elk server for it to monitor them.

A summary of the access policies in place can be found in the table below:

| Name         | Publicly Accessible | Allowed IP Addresses |
|--------------|---------------------|----------------------|
| Jump Box     | Yes from port 22    | 72.50.xxx.xx         |
| ELK NET      | No                  | 10.0.0.5             |
| Web-1,2,3    | NO                  | 10.0.0.5             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows administrators to focus on other tasks and maximize producivity.

The playbook implements the following tasks:

- From host machine SSH into the Jumpbox Provisioner
- Start then attach to docker containers
- cd into /etc/ansible/roles directory to create (nano) elk_server.yml file
- Run the playbook in the container to install
- SSH into ELK_server from the container and run `sudo docker ps` to ensure it was successfully configured

The following screenshot displays the result of running `sudo docker ps` after successfully configuring the ELK instance and the playbook needed to install the ELK machine:

![ELK_Server_Docker_PS](../Ansible/Images/ELK-Server-Docker-PS.png)

![elk_server](../Ansible/elk_server.yml)

### Target Machines & Beats

This ELK server is configured to monitor Web-1, Web-2, and Web-3 VM's at the following respective ip addresses 10.0.0.6, 10.0.0.7, and 10.0.0.9

We have installed the following Beats on these machines:

- Filebeat

- Metricbeat

These Beats allow us to collect the following information from each machine:
Using filebeat we are able to collect and view various log entries such as logins, attempted logins, and VM's turning on. With Metricbeat we are able to view various metrics of the VM's such as CPU usage, Network Traffic, and Memory usage. Metricbeat allows one to view the metrics of a connected network as if looking at those of a host machine in task manager.

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned, here are simple steps to configure the playbooks:

- SSH into the control node
- Navigate to the /etc/ansible/roles directory
- Create the filebeat_config file editing lines 1106 and 1806 to allow the ELK_server ip.
- Run the config file then navigate to (ELK_server private ip:5601) to navigate to Kibana.
- Once on the Kibana site, navigate to the Logs section to verify that logbeat is working and making a connection to Web-1,2,3.
- For Metricbeat, follow the same instructions but creating a metricbeat playbook and editing lines 62 and 96. Also, check that the metricbeat is working and making the connections.
