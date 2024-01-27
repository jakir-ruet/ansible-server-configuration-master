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

Here we are install on Ubuntu Server 22.04 Machine.
``` bash
   sudo apt-get install ansible
```

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
