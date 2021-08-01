# Elk-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](Azure%20Network%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Elk file may be used to install only certain pieces of it, such as Filebeat.

  '''
  ---
     - name: Configure Elk VM with Docker
       hosts: elk
       become: true
       tasks:
       - name: Install docker.io
         apt:
          update_cache: yes
          force_apt_get: yes
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
         value: 262144
         state: present
         reload: yes

      - name: download and launch a docker elk container
        docker_container:
          name: elk
          image: sebp/elk:761
          state: started
          restart_policy: always
          published_ports:
           -  5601:5601
           -  9200:9200
           -  5044:5044

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting traffic to the network.
- Load balancers can protect systems against DDos attacks. The advantage of a jump box is having the ability to SSH into a virtual machine and containers with hardened security.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat monitors multiple logs depending on the area specified?
- Metricbeat records the statistics from the servers.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.5   | Linux            |
| Web-1 VM | Webserver| 10.0.0.6   | Linux            |
| Web-2 VM | Webserver| 10.0.0.7   | Linux            |
| ELK VM   | Webserver| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 40.114.45.74

Machines within the network can only be accessed by the Jump Box VM.
- I allowed my Jump Box to access my Elk VM. The IP address is 10.0.0.5

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 40.114.45.74         |
| Elk VM   | No                  | 10.0.0.5             |
| Web-1 VM | No                  | 10.0.0.5             |
| Web-2 VM | No                  | 10.0.0.5             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows users to save time when their daily tasks are automated.

The playbook implements the following tasks:
- Installs Docker
- Installs python3-pip
- Installs Docker python module
- Sets the vm.max_map_count to 262144
- Downloads and launches a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](Docker%20PS%20Output.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.0.6
- Web-2 10.0.0.7
- Elk VM 10.1.0.4

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors log files and collects log events from specific locations such as the unique visitors logs. Metric beat records the statistics from the operation system running on the server such as how much RAM or CPU was being used at a specific time.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files/filebeat-config.yml.
- Update the filebeat-config.yml file to include the IP address of the Elk Machine.
- Run the playbook, and navigate to http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

- filebeat-playbook.yml is the playbook file and it is copied to Web-1 and Web-2 VMs.
- You will need to update the hosts file to make Ansible run the playbook on a specific machine. You will need to list 10.1.0.4 ansible_python_interpreter=/usr/bin/python3 to install the ELK server on and specify the IP addresses of Web-1 and Web-2 VMs to install Filebeat on.
- I navigated to http://40.78.145.129:5601/app/kibana in order to check that the ELK server is running.
