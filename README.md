# Ansible Configuration Management - Automate Project 7 to 10

What is Ansible? Ansible is an open source, command-line IT automation software application written in Python. It can configure systems, deploy software, and orchestrate advanced workflows to support application deployment, system updates, and more. Ansible’s main strengths are simplicity and ease of use. It also has a strong focus on security and reliability. It uses OpenSSH for transport (with other transports and pull modes as alternatives), and uses a human-readable language that is designed for getting started quickly without a lot of training.

In Projects 7 to 10 we performeda lots of manual operations to set up virtual servers, install and configure required software and deploy your web application.
In this project we will be making most of the routine tasks automated with Ansible Configuration Management, at the same time we will become confident with writing code using declarative languages such as YAML.

### Ansible Client as a Jump Server (Bastion Host)

A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If we think about our current architecture we are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provides better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

![Screenshot 2023-11-15 074236](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/1fe8ee1c-1cb3-4a88-8779-f556fece0407)

## Tasks
* Install and configure Ansible client to act as a Jump Server/Bastion Host
* Create a simple Ansible playbook to automate servers configuration

### Step 1 - Install and Configure Ansible on EC2 Instance

1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.
2. In your GitHub account create a new repository and name it ansible-config-mgt.
3. Install Ansible (see: install Ansible with pip)
