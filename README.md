## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/ELK Stack Network Topology.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - install-elk.yml

---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
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

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

-Load balancers protect the aspect of cybersecurity known as availability.  

-The advantage of a jump box is to provide only one gateway (access point) to the infrastructure, in turn, minimalizing the attack surface.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump box and system network.
  -What does Filebeat watch for? Filebeat watches for changes (new log entries) in  files on a computer
 -What does Metricbeat record? Metricbeat records metrics and statistics from the system and services running. 

The configuration details of each machine may be found below.

| Name     | Function  | IP Address | Operating System |
|----------|---------- |------------|------------------|
| Jump Box | Gateway   | 10.0.0.1   | Linux            |
| Web1     | Webserver | 10.0.0.5   | Linux            |
| Web2     | Webserver | 10.0.0.6   | Linux            |
| ELK      | Monitoring| 10.2.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
 - 73.165.30.229

Machines within the network can only be accessed by jump box provisioner .
   -My personal computer with an external IP address of 73.165.30.229.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |   20.106.122.36      |
| Web1     | No                  |    10.0.0.5          |
| Web2     | No                  |    10.0.0.7          |
| Elk      | No                  |    10.2.0.4          |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
  - It is easy to implement and simple to use.
 - Ansible files are created using YAML which is simple to learn, write, and read.
 - Ansible is agentless, no software is needed to be installed on servers, saving time.
 - Ansible is secure because it uses SSHas its connection method. 

  
The playbook implements the following tasks:
  - Install docker.io
  - Install pip3
  - Install Docker python module
  - Increase virtual memory
  - Use more memory
  - Download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Path of image: Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
   
  - Web1 with the IP addr of 10.0.0.5
  - Web2 with the IP addr of 10.0.0.7

We have installed the following Beats on these machines:
  -Filebeats and Metricbeats

These Beats allow us to collect the following information from each machine:
- Filebeat looks at different types of logs files such as but not limited to server, system, and application logs, collects log events, and then ships the info to ELK.
 -Metricbeats collects metrics and stats about servers and services and ships the info to ELK.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible Control Node .
- Update the host file to include Webserver and ELK.
- Run the playbook, and navigate to http://52.168.121.24:5601/app/kibana#/home to check that the installation worked as expected.

-Which file is the playbook? filebeat-playbook.yml 

-Where do you copy it? /etc/ansible/roles

-Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts file

-How do I specify which machine to install the ELK server on versus which to install Filebeat on? Enter the etc/ansible/hosts file and edit the section labled Webservers (this is the section where the beats will be installed on).  The other sectionis labled Elk which will have the IP address of the ELk server.

-Which URL do you navigate to in order to check that the ELK server is running?  http://52.168.121.24:5601/app/kibana#/home

