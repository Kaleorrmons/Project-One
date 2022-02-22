# Project-One
This is a repository to showcase my skills
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram drawio (1)](https://user-images.githubusercontent.com/90272545/155070602-ff9bc34c-3003-42e8-bc09-fcd8228bf00c.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

 Install_elk.yml

````
---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    Use apt module
    name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

    Use apt module
    name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    Use pip module
    name: Install Docker python module
      pip:
        name: docker
        state: present

    Use sysctl module
    name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    Use docker_container module
    name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

    Use systemd module
    name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
````

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting in-bound access to the network.

- Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpbox and system network.

- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them

- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | LB       | 10.0.0.5   | Linux            |
| Web-2    | LB       | 10.0.0.6   | Linux            |
| Elk      | Server   | 10.2.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 5061 Kibana Port

Machines within the network can only be accessed by jump box provisioner.

- The machine that has access to the ELK VM is my machine with it's IP address of 70.114.147.201.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 70.114.147.201       |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| Elk      | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible automation helps considerably with the representation of Infrastructure as Code (IAC). 


The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Install Docker python module
- Increase virtual memory
- Download and launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/90272545/155060676-63882bb2-7d14-478e-97fe-8cf806ee24ac.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:

Web-1	10.0.0.5
Web-2	10.0.0.6

We have installed the following Beats on these machines:
- Microbeats

These Beats allow us to collect the following information from each machine:
- Filebeat - collects data about the file system
- Metricbeat - collects machine metrics, such as uptime

### Using the Playbook!

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to ancible control node.
 ```
$ cd /etc/ansible
$ mkdir files
# Clone Repository + IaC Files
$ git clone https://github.com/TenkiYamada/Project-1-ELK-Stack-Project.git
# Move Playbooks and hosts file Into `/etc/ansible`
$ cp /Project-1-ELK-Stack-Project/ReadMe/Playbooks/*
```
- Update the hosts file to include webservers and elk.
```
$ cd /etc/ansible
$ cat > hosts <<EOF
[webservers]
10.0.0.5
10.0.0.6

[elk]
10.2.0.4
EOF
```
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

- The filebeat-config.yml is the playbook. You can copy it from: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > filebeat-config.yml

- The file that you update is the 'hosts' file to make ansible run the playbook. You nano into 'hosts' and desgniate which server (webserver or elk) that you want and list their ip address and python. 

- the url that you navigate to in order to check that the ELK server is running is: http://[your.VM.IP]:5601/app/kibana
