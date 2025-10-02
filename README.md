## More About Me – [Take a Look!](http://www.mjakaria.me)

### Ansible

Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality.

Here Comparison of scripts vs Ansible playbooks

| **Feature**        | **Scripts**                                             | **Ansible Playbooks**                     |
| ------------------ | ------------------------------------------------------- | ----------------------------------------- |
| **Language**       | Bash, Python, etc.                                      | YAML                                      |
| **Execution**      | Manual or with a scheduler                              | Automated, run via `ansible-playbook`     |
| **Idempotency**    | Not inherent, must be coded                             | Built-in, automatic idempotency           |
| **Error Handling** | Manual, custom coding required                          | Built-in error handling and reporting     |
| **Reusability**    | Low (hard to reuse across systems)                      | High (roles, variables, playbooks)        |
| **Complexity**     | Can get complex quickly                                 | Modular and structured                    |
| **Scalability**    | Limited scalability, requires manual parallel execution | Scales easily across thousands of systems |

## Basic of Ansible

1. **Tasks:**
   A task is the smallest unit of work within a playbook. A task represents a single action or operation that should be performed on a target host or a group of hosts.

```bash
   tasks:
      - name: Install Apache
         apt:
            name: apache2
            state: present
```

2. **Control Node:**
   Refers to the machine where Ansible is installed and from which you manage and run Ansible tasks and playbooks. This is the system where you write, store, and execute your Ansible playbooks and where the Ansible command-line tools are installed.

3. **Managed Nodes:**
   It is a system or device that Ansible manages and configures. It is the target machine where Ansible executes tasks defined in playbooks.

4. **Inventory:**
   The inventory is a configuration file that defines the hosts and groups of hosts that Ansible will manage. The inventory file specifies the target systems where Ansible playbooks and commands will be executed. It is a crucial component that helps Ansible understand the infrastructure it is working with.

```bash
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

```INI
[web_servers]
server1 ansible_host=192.168.1.101
server2 ansible_host=192.168.1.102

[database_servers]
db_server ansible_host=192.168.1.103
```

5. [**Modules:**](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html)
   Is a small scripts or programs that carry out specific tasks on the target hosts, here `copy:` is a module. They can be written in various languages such as Python, PowerShell, Ruby, and more. Ansible modules are responsible for handling various aspects of system configuration and management, such as installing packages, managing files, starting services, and more on remote node. Some modules shown below list;
      - System Modules (user, group, service, firewalld, hostname, timezone etc.)
      - Cloud Modules (ec2, ec2_vpc, ec2_elb, ec2_key, ec2_ami etc.)
      - Clustering Modules (k8s, k8s_facts, docker_container, docker_service etc.)
      - Database Modules (mysql_db, mysql_user, mongodb, mongodb_user etc.)
      - Messaging Modules (rabbitmq_user, rabbitmq_vhost, rabbitmq_binding etc.)
      - Inventory Modules (host, group, ini_file, yaml_inventory etc.)
      - Commands Modules (command, shell, script etc.) etc.

```bash
- name: Copy a file to remote hosts
   hosts: my_servers
   tasks:
      - name: Copy file
         copy:
         src: /path/to/local/file.txt
         dest: /path/on/remote/file.txt
```

6. **Playbooks:**
   Ansible playbook is a structured configuration file used to define a set of tasks and automate the execution of those tasks on a group of hosts. Playbooks are written in YAML. A playbook consists of one or more plays, where each play defines a set of tasks to be executed on a specified group of hosts.

```bash
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

7. **Ad-Hoc Command:**
   An ad-hoc command is a one-time command that you run from the command line, without the need to create a playbook. Ad-hoc commands are useful for performing quick tasks, checking the status of systems, or making immediate changes across a group of hosts.

```bash
ansible <pattern> -i <inventory> -m <module> -a "<module_arguments>"
```

- pattern specifies the hosts/groups of hosts to which the command should be applied.
- `-i` is inventory
- `-m` is module
- `-a` is module_arguments

```bash
ansible web_servers -i inventory_file -m command -a "uptime"
```

8. **API:**
   Here API is a special type of modules that work as transport in cloud services such as Python API.

9. **Plugin:**
   Plugins are modular pieces of code that add functionality to various parts of the Ansible execution process. They enhance the core functionality of Ansible by providing additional capabilities or by allowing you to customize and extend certain aspects of the automation process such as cache plugin, action plugin, callback plugin.

### Vault

A Vault is a feature that lets you `encrypt sensitive` data (like passwords, private keys, API tokens, etc.) so they can be safely stored in playbooks, roles, or variable files without exposing secrets in plain text.

#### Key Points about Ansible Vault

- Encrypts YAML files or even individual variables.
- Uses AES256 encryption by default.
- Requires a password or key file to decrypt and use secrets during playbook execution.
- Keeps your sensitive information safe in version control (Git, etc.).

#### Common vault commands

| Example                                                            | Description                                    |
| ------------------------------------------------------------------ | ---------------------------------------------- |
| `ansible-vault create secrets.yaml`                                | Create a new encrypted file                    |
| `ansible-vault encrypt vars.yaml`                                  | Encrypt an existing file                       |
| `ansible-vault decrypt secrets.yaml`                               | Decrypt an encrypted file back to plain text   |
| `ansible-vault edit secrets.yaml`                                  | Open and edit an encrypted file in editor      |
| `ansible-vault view secrets.yaml`                                  | View contents of an encrypted file (read-only) |
| `ansible-vault rekey secrets.yaml`                                 | Change the vault password for a file           |
| `ansible-playbook site.yaml --ask-vault-pass`                      | Run a playbook with vault password prompt      |
| `ansible-playbook site.yaml --vault-password-file .vault_pass.txt` | Run a playbook using a password file           |

#### How it works

![Ansible Project!](/img/ansible-project.png 'ansible-project')

#### Ansible running two ways, Here are the primary ways Ansible works:

- Ansible Command (Ad-Hoc Commands) - Imperative Approach
- Ansible Playbook Command - Declarative Approach

#### Architecture

![Ansible Architecture!](/img/ansible-architecture.png 'ansible-architecture')

[CMDB: Configuration Management DataBase]

#### Lab Setup

By following some steps lab setup will be done.

- Collect operating `VDI` file following website [**OSBoxes**](https://www.osboxes.org/)
- Making a Operating System Template as per requirements
- Create Ansible Controller Machine
- Create Ansible Target1 Machine
- Create Ansible Target2 Machine
- Install MobaXterm following website [**MobaXterm**](https://mobaxterm.mobatek.net/)
- Connect three machine into MobaXterm.

#### Install Process

Here we are installing only on Ubuntu Server 24.04 Machine. We need three server VMs one is an Ansible server & other two work as nodes.

```bash
apt update
apt install software-properties-common
apt-add-repository --yes --update ppa:ansible/ansible
apt install ansible
ansible --version
```

Active the bash completion support we may install these.

```bash
sudo apt update
sudo apt install python3-argcomplete -y
activate-global-python-argcomplete
eval "$(register-python-argcomplete ansible)"
source ~/.bashrc
```

To allow nodes into the ansible server, create a group on the 'hosts' file. A group shown as follows;

```bash
nano /etc/ansible/hostname
nano /etc/ansible/hosts
# Try login target machine, server machine IP 192.168.68.165
ssh 192.168.68.166
```

Now let's check using inventory

- Create directory `mkdir test-project` in server machine.
- Create `inventory.txt` in `test-project` directory.
- Write these on `inventory.txt`

```bash
target1 ansible_host=192.168.68.166 ansible_ssh_pass=054003
target2 ansible_host=192.168.68.167 ansible_ssh_pass=054003
```

Let's check ping test. Here getting failed message. If change (just un-comment) `host_key_checking = False` in `ansible.cfg`. But its not `recommended` way.

```bash
ansible target1 -m ping -i inventory.txt
ansible target2 -m ping -i inventory.txt
ansible all -m ping -i inventory.txt
ansible target1 -i inventory.txt -m command -a "uptime"
```

Here, IP & Password sync is recommended way.

Let's try another way

```bash
[AnsibleGroup] # Group Name
192.168.68.166 # Node #1 IP
192.168.68.167 # Node #2 IP
```

And update the ansible.cfg file

```bash
nano /ete/ansible/ansible.cfg
[defaults]
inventory = /etc/ansible/hosts # comment out or add
sudo_user = root # comment out or add
```

Create a user (same) in three instances

```bash
adduser ansible-usr
passwd 054003
```

To give 'sudo privileges' to 'ansible-usr' & put this line below **%admin** user of three instances

```bash
sudo visudo
%ansible-usr ALL=(ALL) NOPASSWD:ALL
usermod -aG docker ansible-usr # add user to docker group
```

Login as **ansible-usr** user into three instances

```bash
su - ansible-usr
sudo apt-get update
```

Give the permissions on three nodes under user (not ansible-usr).

```bash
sudo nano /etc/ssh/sshd_config
PubkeyAuthentication yes
PasswordAuthentication yes
service ssh restart
service ssh status
```

Login as 'ansible-usr' into three instances.

```bash
su - ansible-usr # in server
```

Try to connect node instances form ansible server instances under 'ansible-usr'.

```bash
ssh 192.168.68.166 # Node #1 IP
```

It is asking password for each login, which is very disturbing. under the 'ansible-usr' of the server we will disable the password in all node instances by generating ssh-key.

```bash
ssh-keygen # does not use the password
ls -a
cd .ssh # show keygen only
ssh-copy-id ansible-usr@172.16.102.132 # from server instance (here 172.16.102.132 is node 1 private IP)
ssh-copy-id ansible-usr@172.16.102.133 # from server instance (here 172.16.102.133 is node 2 private IP)
ssh-copy-id localhost # if need
su - ansible-usr # in server
```

#### Ad-Hoc

Checking the all connected node

```bash
ansible all -m ping
```

Checking specific node group

```bash
ansible ansibleGroup -m ping
```

Checking specific node groups and specific nodes using index

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

Check the OS memory.

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

```bash
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
      update_cache: yes
      state: latest
  - name: add php support for apache
    apt:
      name: libapache2-mod-php
      update_cache: yes
      state: latest
```

Check the playbook work or not.

```bash
ansible-playbook install_apache.yaml --check
```

Install apache to all nodes

```bash
ansible-playbook --ask-become-pass install_apache.yaml
```

```bash
sudo nano remove_apache.yaml
```

Write a playbook for apache remove.

```bash
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
  - name: remove php support for Apache
    apt:
      name: libapache2-mod-php
      state: absent
```

Remove Apache to all nodes

```bash
ansible-playbook --ask-become-pass remove_apache.yaml
```

It has been installed successfully try to see to the browser. You will show the default page of Apache.

#### [Variable](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)
Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including **lists** and **dictionaries**. You can define these variables

- In your playbooks,
- In your inventory,
- In reusable files or roles, or
- At the command line.

**NB:** You can also create variables during a playbook run by registering the return value or values of a task as a new variable.

**Valid variable names**

- Only letters (a...z, A...Z), numbers (1-9) & underscore (_) use in variable.
- Variable may begin with underscore (_)
- Such as myvar, my_var, _my_var.

Checking system information

```bash
ansible all -m setup
```

Checking specific system information

```bash
ansible AnsibleGroup -m setup -a 'filter=ansible_hostname'
```

##### First variable **in playbook**
Create the first var in a playbook

```bash
nano var-apache-playbook.yaml
```

Writing this YAML code in `var-apache-playbook.yaml`

```bash
---
- hosts: all
  vars:
   - username: Jakir
   - designation: IT Manager
  tasks:
   - name: my first variable in a playbook
     debug:
       msg: "Your name is {{username}}, Designation is {{designation}}"
```

Run the var in the playbook not mention the inventory

```bash
ansible-playbook var-apache-playbook.yaml
```

Or Run the var in the playbook with mention of the inventory & username

```bash
ansible-playbook -i inventory var-apache-playbook.yaml -u jakir
```

##### First var as a new file/files
Create a var named is `vars.yaml`

```bash
nano vars.yaml
```

Type/Paste on `vars.yaml` this line

```bash
package_name: apache2
```

Create a playbook named `install-apache.yaml` and type these yaml code.

```bash
---
- name: Apache Web Server Install
  hosts: all
  become: true

  tasks:
  - include_vars: vars.yaml
  - name: install apache2
    apt:
      name={{package_name}}
      update_cache=yes
      state=latest
```

Run the var in playbook not mention the inventory

```bash
ansible-playbook install-apache.yaml
```

Or Run the var in playbook with mention the inventory & username

```bash
ansible-playbook -i inventory install-apache.yaml -u jakir
```

##### [Loops](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html#iterating-over-a-simple-list)
Loops in Ansible are like shortcuts for doing the same task many times. Instead of repeating the same steps over and over again, we use a loop to do the task as many times as we need.
Create a var named is `simple-loop.yaml`

```bash
nano vars.yaml
```

```bash
---
- name: Name of simple list
  hosts: all
  gather_facts: no
  become: true
  tasks:
    - name: Add services
      user:
         name: "{{ item }}" # call the from loop
         state: present
      loop:
         - user1
         - user2
         - user3
```

Run the var in playbook not mention the inventory

```bash
ansible-playbook simple-loop.yaml
```

Or Run the var in playbook with mention the inventory & username

```bash
ansible-playbook -i inventory simple-loop.yaml -u jakir
```

```bash
ansible AnsibleGroup -m shell -a 'tail -3 /etc/passwd' # here 3 is number of user
```

**Other loop such as hash, dictionary see in '03-loops' directory**

## With Regards, `Jakir`

[![LinkedIn][linkedin-shield-jakir]][linkedin-url-jakir]
[![Facebook-Page][facebook-shield-jakir]][facebook-url-jakir]
[![Youtube][youtube-shield-jakir]][youtube-url-jakir]

### Wishing you a wonderful day! Keep in touch.

<!-- Personal profile -->

[linkedin-shield-jakir]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-jakir]: https://www.linkedin.com/in/jakir-ruet/
[facebook-shield-jakir]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-jakir]: https://www.facebook.com/jakir.ruet/
[youtube-shield-jakir]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-jakir]: https://www.youtube.com/@mjakaria-ruet/featured
