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

![Screenshot 2023-09-11 085657](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/67745949-1385-484b-903f-dc78c47b3d9b)

3. Install Ansible (see: install Ansible with pip)
```
sudo apt update
sudo apt install ansible
Check your Ansible version by running ansible --version
```

![Screenshot 2023-09-11 090938](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/9c6d9bbf-65fe-493b-8ab6-ba8b9e5dedd1)

4. Configure Jenkins build job to archive your repository content every time you change it - this will solidify our Jenkins configuration skills acquired in Project 9.

* We will create a new Freestyle project ansible in Jenkins and point it to our 'ansible-config-mgt' repository.

![Screenshot 2023-09-11 102024](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/3825436f-31aa-4781-b9d3-2985d9fc7c20)

*   we will configure a webhook in GitHub and set the webhook to trigger ansible build.
  
![Screenshot 2023-09-11 095339](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/c7d6ca3b-eaf9-49fb-9978-387b9dff0bd0)

![Screenshot 2023-09-11 101347](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/7701b971-0fb4-4a7e-b564-09890c70a1f9)

* Configure a Post-build job to save all (**) files, like you did it in Project 9.

![Screenshot 2023-09-11 102623](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/b52f5624-1710-45eb-936c-0bb79724f49f)

5. Now we are going to test our setup by making some change in README.md file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

```
ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```
![Screenshot 2023-09-11 103614](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/88c9416b-6c25-4636-a434-a3fb0e127218)

![Screenshot 2023-09-11 110114](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/f426fffc-7f87-44ac-a0b4-8cf6c507cecb)

Note: Every time we stop/start our Jenkins-Ansible server - we have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server.

## Step 2 - Prepare your development environment using Visual Studio Code

1. First part of 'DevOps' is 'Dev', which means we be will required to write some codes and we shall have proper tools that will make our coding and debugging comfortable - we need an Integrated development environment (IDE) or Source-code Editor.
2. After we have successfully installed VSC, we will configure it to connect to our newly created GitHub repository.
3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

```
git clone <ansible-config-mgt repo link>
```

## Step 3 - Begin Ansible Development

1. In our ansible-config-mgt GitHub repository, we will create a new branch that will be used for development of a new feature.
2. Lets checkout the newly created feature branch to our local machine and start building our code and directory structure

![Screenshot 2023-09-13 071919](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/74811925-6ae2-4295-91d1-20efd8fa970a)

We use git checkput -b command to create and checkout to our local machine

3. We will create a directory and name it playbooks - it will be used to store all our playbook files.

![Screenshot 2023-09-13 072016](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/09f04b47-1117-4e6b-a5fb-398a86db515d)

4. We will create a directory and name it inventory - it will be used to keep our hosts organised.

![Screenshot 2023-09-13 072315](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/9d5ffbd2-bb42-46a3-97b8-5ee9af553d43)

5. Within the playbooks folder, create your first playbook, and name it common.yml

![Screenshot 2023-09-13 075340](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/9e18e8d3-805f-4f07-8a81-388eec603c8b)

6. Within the inventory folder, create an inventory file () for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively. These inventory files use .ini languages style to configure Ansible hosts.

![Screenshot 2023-09-13 075340](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/c5b36617-e16d-4c67-9175-bb352a5faea7)

## Step 4 - Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save the below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers  from Jenkins-Ansible host - for this we can implement the concept of ssh-agent. Now we need to import our key into ssh-agent:

```
eval `ssh-agent -s`
ssh-add <path-to-private-key>
```

![Screenshot 2023-09-14 141559](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/409d8a8f-71e4-4ff8-a6c9-5032e7c84190)

Let's confirm the key has been added with the command below, we should see the name of our key

```
ssh-add -l 
```
![Screenshot 2023-09-14 141653](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/bb1640e4-f68c-40bb-8966-b14184450aec)

Now, let's ssh into our Jenkins-Ansible server using ssh-agent

```
ssh -A ubuntu@public-ip
```
![Screenshot 2023-09-14 143446](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/fbac346e-011c-408f-b6d3-74a7f79e6b08)

Now let's update your inventory/dev.yml file with this snippet of code:

```
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user
<Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user

[db]
<Database-Private-IP-Address> ansible_ssh_user=ec2-user 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu
```

![Screenshot 2023-09-13 082443](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/4fd60362-a65c-43c3-aba2-e577ddecac2a)

## Step 5 - Create a Common Playbook

It is time to start giving Ansible the instructions on what you need to be performed on all servers listed in inventory/dev. In common.yml playbook we will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   

- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```

![Screenshot 2023-09-13 082443](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/0b2ebee7-a252-4be5-84bf-698c3bac686c)

This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

## Step 6 - Update GIT with the latest code

Now all of our directories and files live on your machine and we will need to push changes made locally to GitHub.

1. Use git commands to add, commit and push your branch to GitHub.

```
git status

git add <selected files>

git commit -m "commit message"
```
![Screenshot 2023-09-15 062728](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/6864f03a-38a3-4822-a18e-5490fdfdefe9)

![Screenshot 2023-09-15 064709](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/dcc68345-0892-4f94-a2f2-1ffc4197faae)

2. Create a Pull request (PR)

![Screenshot 2023-09-15 065959](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/d6f1cfdb-f9ca-4f8a-be07-d63e3d67c54f)

3. since we have permission to the master branch, we can go ahead merge the code to the master branch. if our merging is successful we are going to see our code in master branch (in the image below)

![Screenshot 2023-09-15 071824](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/3ad3b330-774a-4354-99ba-9df47c8ff63b)

4. Once our code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server

![Screenshot 2023-09-15 071845](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/625d10a6-d9ee-4069-bb47-0d5040db951d)

![Screenshot 2023-09-15 074018](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/522ef79e-0dbe-43bd-a5ba-6c9947eb8c29)

## Step 7 - Run first Ansible test

1. Setup your VSCode to connect to your instance as demonstrated by the video above. Now run your playbook using the command:

```
cd ansible-config-mgt
ansible-playbook -i inventory/dev.yml playbooks/common.yml
```
Note: We must make sure we are in our ansible-config-mgt directory before runing the above command.

![Screenshot 2023-09-15 101648](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/746c4787-968b-4fca-bb71-93bbd8582835)

The image above shows we have just automated our routine tasks by implementing our first Ansible project!

Now our architecture looks like the image below:

![image](https://github.com/opeyemiogungbe/Pbl-project9/assets/136735745/4a7344cf-2c05-4ce4-be7e-7a97e115338f)

