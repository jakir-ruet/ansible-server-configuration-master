[![Youtube][youtube-shield]][youtube-url]
[![Facebook-Page][facebook-shield]][facebook-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

## Visit Us [Lapis Soft](http://www.lapissoft.com)

### Ansible

Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality.

## Basic of Ansible

   1. ***Tasks:***
      A task is the smallest unit of work within a playbook. A task represents a single action or operation that should be performed on a target host or a group of hosts.
      ``` YAML Format
         tasks:
            - name: Install Apache
              apt:
                 name: apache2
                 state: present
      ```
   2. ***Control Node:***
      Refers to the machine where Ansible is installed and from which you manage and run Ansible tasks and playbooks. This is the system where you write, store, and execute your Ansible playbooks and where the Ansible command-line tools are installed.
   3. ***Managed Nodes:***
      It is a system or device that Ansible manages and configures. It is the target machine where Ansible executes tasks defined in playbooks.
   4. ***Inventory:***
      The inventory is a configuration file that defines the hosts and groups of hosts that Ansible will manage. The inventory file specifies the target systems where Ansible playbooks and commands will be executed. It is a crucial component that helps Ansible understand the infrastructure it is working with.
      ``` YAML Format
         all:
            hosts:
               server1:
                  ansible_host: 192.168.1.101
               server2:
                  ansible_host: 192.168.1.102
            children:
               database_servers:
                  hosts:
                  db_server:
                     ansible_host: 192.168.1.103
      ```

      ``` INI Format
         [web_servers]
         server1 ansible_host=192.168.1.101
         server2 ansible_host=192.168.1.102

         [database_servers]
         db_server ansible_host=192.168.1.103
      ```
   5. ***Modules:***
      Is a small scripts or programs that carry out specific tasks on the target hosts. They can be written in various languages such as Python, PowerShell, Ruby, and more. Ansible modules are responsible for handling various aspects of system configuration and management, such as installing packages, managing files, starting services, and more.

      ``` YAML Format
         - name: Copy a file to remote hosts
           hosts: my_servers
           tasks:
              - name: Copy file 
                copy:
                  src: /path/to/local/file.txt
                  dest: /path/on/remote/file.txt
      ```
   6. ***Playbooks:***
      Ansible playbook is a structured configuration file used to define a set of tasks and automate the execution of those tasks on a group of hosts. Playbooks are written in YAML. A playbook consists of one or more plays, where each play defines a set of tasks to be executed on a specified group of hosts.

      ``` YAML Format
         - name: Configure Web Servers
           hosts: web_servers
           become: true  # Run tasks with elevated privileges (sudo)

           tasks:
               - name: Ensure Apache is installed
                 apt:
                    name: apache2
                    state: present

               - name: Ensure Apache service is running
                 service:
                    name: apache2
                    state: started

               - name: Copy index.html to the web server
                 copy:
                    src: /path/to/local/index.html
                    dest: /var/www/html/index.html
      ```
      
   7. ***Ad-Hoc Command:***
      An ad-hoc command is a one-time command that you run from the command line, without the need to create a playbook. Ad-hoc commands are useful for performing quick tasks, checking the status of systems, or making immediate changes across a group of hosts.
      Syntax
      ``` bash
         ansible <pattern> -i <inventory> -m <module> -a "<module_arguments>"
      ```
      - pattern is Specifies the hosts/groups of hosts to which the command should be applied.
      - -i is inventory
      - -m is module
      - -a is module_arguments

      Example
      ``` bash
         ansible web_servers -i inventory_file -m command -a "uptime"
      ```
   8. ***API:***
      Here API is special types of modules that work as transport in cloud services such Python API.
   9. ***Plugin:***
      Plugins are modular pieces of code that add functionality to various parts of the Ansible execution process. They enhance the core functionality of Ansible by providing additional capabilities or by allowing you to customize and extend certain aspects of the automation process such as cache plugin, action plugin, callback plugin.

#### How to works

![Ansible Project!](/img/ansible-project.png 'ansible-project')

#### Architecture

![Ansible Architecture!](/img/ansible-architecture.png 'ansible-architecture')

[CMDB: Configuration Manangement DataBase]

#### Install Process

Here we are installing on Ubuntu Server 22.04 Machine. We need three server VMs one is an Ansible server & other two work as nodes.
``` bash
add-apt-repository ppa:ansible/ansible
```
``` bash
sudo apt-get install ansible
```

``` bash
ansible --version
```
Active the bash completion support we may install these.
``` bash
apt install python3-argcomplete
activate-global-python-argcomplete3
```
To allow nodes into the ansible server, create a group on the 'hosts' file. A group show as follows;
``` bash
nano /etc/ansible/hosts
```
``` bash
[AnsibleGroup] # Group Name
172.16.102.130 # Node #1 IP
172.16.102.131 # Node #2 IP
```
And update the ansible.cfg file
``` bash
nano /ete/ansible/ansible.cfg
```
Add/Update the following line in ansible.cfg file.
``` bash
[defaults]
inventory = /etc/ansible/hosts
sudo_user = root
```
Create a user in three instances
``` bash
adduser ansible-usr
passwd 054003
```
To give 'sudo privillages' to 'ansible-usr' user in node instances.
``` bash
sudo visudo
```
And put this line below %admin user of three instances
```bash
%ansible-usr ALL=(ALL) NOPASSWD:ALL
```
Login as ansible-usr user into three instances
```bash
su - ansible-usr
```
```bash
sudo apt-get update
```
Try to connect node instances
```bash
ssh 172.16.102.130 # Node #1 IP
```
Here, access is denied, or a password is required. For this purpose, we uncomment/change the sshd_config file on three instances. It must be done under the root user of these instances.
```bash
sudo nano /etc/ssh/sshd_config
```
```bash
PubkeyAuthentication yes
PasswordAuthentication yes
```
Restart the ssh service & check status
```bash
service ssh restart
```
```bash
service ssh status
```
Login as 'ansible-usr' into ansible server.
```bash
su - ansible-usr # in server
```
Try to access (two) node instances from the ansible server under the 'ansible-usr'. This is very disturbing because it is asking for the pass for each login.
```bash
ssh 172.16.102.130 # node IP
```

Disable the password asking each time while logging the ansible server (ansible-usr) to the node instance by generating ssh-key.
```bash
ssh-keygen # not use the password
ls -a
cd .ssh
ssh-copy-id ansible-usr@172.16.102.132 # from server instance (here 172.16.102.132 is node 1 IP)
ssh-copy-id ansible-usr@172.16.102.133 # from server instance (here 172.16.102.133 is node 2 IP)
```

#### Ad-Hoc
Checking the all connected node
```bash
ansible all -m ping
```
Checking specific node group
```bash
ansible AnsibleGroup -m ping
```
Checking specific node group and specific node using index
```bash
ansible AnsibleGroup[0] -m ping
```

##### File operations
Checking available hosts
```bash
ansible all --list-hosts
```
Checking all connected node's information
```bash
ansible all -m gather_facts
```
Checking specific connected node's information
```bash
ansible all -m gather_facts --limit NodeIPAddress
```
Checking files/directories
```bash
ansible AnsibleGroup -a "ls"
```
Create a file on all connected node
```bash
ansible all -a "touch black_berry.txt"
```
Create a file only selected node
```bash
ansible AnsibleGroup[0] -a "touch black_berry1.txt"
```
Create a file on ansible server
```bash
touch demo.txt
```
Copy this file and paste to ansible node 1.
```bash
ansible AnsibleGroup[0] -m copy -a "src=./demo.txt dest=/tmp/demo.txt"
```
See the in detail of connected node
```bash
ansible AnsibleGroup -m setup
```
Check the OS Info as as OS family.
```bash
ansible AnsibleGroup -m setup -a 'filter=ansible_os_family'
```
Check the OS momory.
```bash
ansible AnsibleGroup -m setup -a 'filter=ansible_os_mb'
```
CPU use process
```bash
ansible AnsibleGroup -m shell -a "top -c -b | head -10"
```
Memory consume process
```bash
ansible AnsibleGroup -m shell -a "ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem | head"
```

#### Playbook
```bash
sudo nano install_apache.yaml
```
Write a playbook for apache install.
```YAML
---
- name: Apache Web Server Install
  hosts: all
  become: true
  tasks:
  - name: update repository index
    apt:
      update_cache: yes
  - name: install apache2 package
    apt: 
      name: apache2
      state: latest
  - name: add php support for apache
    apt:
      name: libapache2-mod-php
      state: latest
```
Check the playbook work or not.
```bash
ansible-paybook install_apache.yaml --check
```
Install apache to all nodes
```bash
ansible-playbook --ask-become-pass install_apache.yaml
```
```bash
sudo nano remove_apache.yaml
```
Write a playbook for apache remove.
```YAML
---
- name: Apache Web Server Remove
  hosts: all
  become: true
  tasks:
  - name: remove repository index
    apt:
      update_cache: yes
  - name: remove apache2 package
    apt: 
      name: apache2
      state: absent
  - name: remove php support for apache
    apt:
      name: libapache2-mod-php
      state: absent
```
Remove apache to all nodes
```bash
ansible-playbook --ask-become-pass remove_apache.yaml
```

It has been installed successfully try to see to the browser. You will show the default page of Apache.
### Courtesy of Jakir

LinkedIn [Profile](https://www.linkedin.com/in/jakir-ruet/),
Facebook [Profile](https://www.facebook.com/jakir.ruet),
GitHub [Profile](https://github.com/jakir-ruet),
Skype [Profile](https://web.skype.com/?openPstnPage=true)

### Have a good day, stay with me

[youtube-shield]: https://img.shields.io/badge/-Youtube-black.svg?style=flat-square&logo=youtube&color=blue&logoColor=red
[youtube-url]: https://www.youtube.com/@LapisSoft/featured
[facebook-shield]: https://img.shields.io/badge/-Facebook-black.svg?style=flat-square&logo=facebook&color=pink&logoColor=blue
[facebook-url]: https://www.facebook.com/GoLapisSoft/
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=red
[linkedin-url]: https://www.linkedin.com/company/lapis-soft/
