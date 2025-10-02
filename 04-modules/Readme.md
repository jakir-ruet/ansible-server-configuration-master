### [Modules:](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html)

Is a small scripts or programs that carry out specific tasks on the target hosts, here `copy:` is a module. They can be written in various languages such as Python, PowerShell, Ruby, and more. Ansible modules are responsible for handling various aspects of system configuration and management, such as installing packages, managing files, starting services, and more on remote node. Some modules shown below list;

- System Modules (user, group, service, firewalld, hostname, timezone etc.)
- Cloud Modules (ec2, ec2_vpc, ec2_elb, ec2_key, ec2_ami etc.)
- Clustering Modules (k8s, k8s_facts, docker_container, docker_service etc.)
- Database Modules (mysql_db, mysql_user, mongodb, mongodb_user etc.)
- Messaging Modules (rabbitmq_user, rabbitmq_vhost, rabbitmq_binding etc.)
- Inventory Modules (host, group, ini_file, yaml_inventory etc.)
- Commands Modules (command, shell, script etc.) etc.
