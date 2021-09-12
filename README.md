## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/83977068/132997729-8b7f9460-756b-459e-afe8-b4819728f431.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

ansible playbook
  

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaiable, in addition to restricting traffic to the network.

What aspect of security do load balancers protect?
Answer: Availabilty, Web Traffic, Web Security

What is the advantage of a jump box?
Answer: Automation, Security, Network Segmentation, Access Control

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
What does Filebeat watch for?
Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

What does Metricbeat record?
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web1    | Web server         | 10.0.0.7           | Linux |
| Web2     | Web server         | 10.0.0.6           | Linux|
| Elk     |  ELK server        |  10.1.0.4 / 40.80.156.117 |  Linux|
| Load balancer| Load Balancer | Static External IP | Linux |
| Work Station| Access Control | Public IP | Linux 
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Red Team Public IP through TCP 5601

Machines within the network can only be accessed by Jump Box Provisnioner.
Jump-Box-Provisioner IP : 10.0.0.4 via SSH port 22


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |No                   | Workstation Public IP on SSH 22 |
|  Web1    | No                  | 10.0.0.7 on SSH 22              | 
|   Web2   | No                  | 10.0.0.6 on SSH 22              |
| ELK Server| No                 | Workstation Public IP using 5601|
| Load Balancer | No             | Workstation Public IP on HTTP 80|
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible allows you to quickly and easily deploy multitier apps. You don't need to write custom code to automate your systems. Just list the tasks required by writing a playbook, and Ansible will figure out how to get the systems where you want them.
It saves time, so you dont have to spend hours configuring machines mannually. 

The playbook implements the following tasks:

Specify a different group of machines as well as a different remote user
 
 - name: Config elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:

Increase System Memory :
 - name: Use more memory
  sysctl:
    name: vm.max_map_count
    value: '262144'
    state: present
    reload: yes

Install the following services:
   `docker.io`
   `python3-pip`
   `docker`, which is the Docker Python pip module.

Launching and Exposing the container with these published ports:
 `5601:5601` 
 `9200:9200`
 `5044:5044`

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/83977068/132997811-4c549009-0973-4063-8b64-76e4d7c3dd36.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web1 : 10.0.0.7
Web2 : 10.0.0.6

We have installed the following Beats on these machines:
ELK Server, Web1 and Web2
The ELK Stack Installed are: FileBeat and MetricBeat

These Beats allow us to collect the following information from each machine:
Filebeat: log events
Metricbeat: metrics and system statistics

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ansible_elk file to Config WebVM with Docker.
- Update the filebeat-playbook.yml file to include installer 
- curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
- Run the playbook ansible-playbook filebeat-playbook.yml, and navigate to  Kibana > Logs : Add log data > System logs > 5:Module Status > Check data to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?

 For the ANSIBLE : We will create the my-playbook1.yml as our playbook.

For FILEBEAT: We will create filbeat-playbook.yml as our playbook.

For METRICBEAT: We will create metricbeat-playbook.yml as our playbook.

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

- root@cf00a05f9647:/etc/ansible# curl -L -O https://ansible.com/  > ansible.cfg
- root@cf00a05f9647:/etc/ansible# nano ansible.cfg

- Press CTRL + W (to search > enter remote_user then change `remote_user = sysadmin`

- _Which URL do you navigate to in order to check that the ELK server is running?
Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana
Test Kibana on localhost: sysadmin@10.1.0.4: curl localhost:5601/app/kibana

Other Linux Command List :

sudo apt-get update	this will update all packages

sudo apt install docker.io	install docker application

sudo service docker start	start the docker application

systemctl status docker	status of the docker application

sudo docker pull cyberxsecurity/ansible	download the docker file

sudo docker run -ti cyberxsecurity/ansible bash	run and create a docker image

sudo docker start <image-name>	starts the image specified

sudo docker ps -a	list all active/inactive containers

sudo docker attach <image-name>	effectively sshing into the ansible

ssh-keygen	create a ssh key

ansible -m ping all	check the connection of ansible containers
