An **Ansible Role** is a reusable and self-contained unit of automation that allows you to manage tasks, variables, files, templates, handlers, and more, in a structured and modular way. Roles are intended to encapsulate the logic and configuration required to manage a specific component of a system or an application.

By organizing your playbooks into roles, you achieve better **modularity**, **reusability**, and **maintainability**, and you ensure a consistent approach to managing infrastructure across different environments.

For role base configuration visit this website.
[**Galaxy**](https://galaxy.ansible.com/ui/)

Create a role
```bash
ansible-galaxy role init test-role
```

## Key Components of an Ansible Role

An Ansible role consists of the following key components:

1. **Tasks**  
   The core actions or steps that the role performs. These are typically defined in the `tasks/main.yaml` file and include actions like installing packages, starting services, or configuring files.

2. **Handlers**  
   Handlers are tasks that are triggered by changes in other tasks. These are typically used to restart services, reload configurations, or take some other action based on changes made in previous tasks. Handlers are defined in the `handlers/main.yaml` file.

3. **Files**  
   This directory contains static files that need to be transferred to managed hosts. For example, configuration files or scripts that must be placed on the target system.

4. **Templates**  
   Jinja2 templates that can be dynamically rendered with variables and then transferred to managed hosts. These templates allow for the generation of configuration files, scripts, and other content based on the host's specific environment.

5. **Vars**  
   Variables specific to the role. These are typically defined in the `vars/main.yaml` file and can be used to configure the behavior of the tasks in the role.

6. **Defaults**  
   Default variables for the role, stored in the `defaults/main.yaml` file. These can be overridden in the playbook, inventory, or by other higher-priority sources.

7. **Meta**  
   Contains metadata about the role, such as role dependencies, author information, and other information. This file is stored in the `meta/main.yaml` file.

8. **Library**  
   Custom Ansible modules or plugins that are used within the role. If the role needs specialized functionality not provided by standard Ansible modules, custom modules can be placed here.

9. **Module_defaults**  
   Default parameters for modules used in the role. This allows you to set default arguments that modules in the role should use.

10. **Lookup_plugins**  
    Custom lookup plugins that are used within the role. Lookup plugins allow you to retrieve data from external sources, such as databases or APIs.

## Directory Structure of an Ansible Role
Here is the standard directory structure for an Ansible role:
```bash
<role_name>/
  ├── defaults/
  │   └── main.yaml
  ├── files/
  ├── handlers/
  │   └── main.yaml
  ├── meta/
  │   └── main.yaml
  ├── tasks/
  │   └── main.yaml
  ├── templates/
  ├── vars/
      └── main.yaml
```