# Ansible Apache Project

This project demonstrates how to use **Ansible** to install and configure an Apache web server on an Amazon Linux 2 instance.

## Overview

Ansible is a powerful automation tool used to configure systems and deploy applications. This project includes:
- Automated **installation** of Apache
- Creation of a **simple HTML file**
- **Starting and enabling** the Apache service
- **Configuring the firewall** to allow HTTP traffic

## Project Setup

### Prerequisites
- **Ansible** installed on your local machine
- **Amazon Linux 2 instance** on AWS
- **SSH access** configured to the instance (using private key)

## Inventory File (`hosts.ini`)

The inventory file defines the target server for Ansible to manage.

[webservers] <your-instance-public-ip> ansible_user=ec2-user


## Playbook File (`install_apache.yml`)

The playbook automates these tasks:
1. Updates the system package manager (YUM)
2. Installs Apache
3. Creates a simple HTML file
4. Starts and enables Apache service
5. Configures the firewall to allow HTTP traffic

```yaml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Update the package manager
      yum:
        name: '*'
        state: latest

    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create a simple HTML file
      copy:
        content: "<h1>Hello, World!</h1>"
        dest: /var/www/html/index.html

    - name: Ensure firewall allows HTTP
      firewalld:
        service: http
        permanent: yes
        state: enabled


*How to Run the Project*
Clone the repository:

git clone https://github.com/rhysandofvelaris/ansible-apache-project.git
cd ansible-apache-project
*Run the playbook:*

ansible-playbook -i hosts.ini install_apache.yml
*Verify the Installation:*

*Open your web browser and go to:*
http://<your-instance-public-ip>
**You should see Hello, World! displayed.**
