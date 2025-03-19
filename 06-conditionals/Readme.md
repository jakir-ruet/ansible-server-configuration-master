In **Ansible**, conditionals allow you to control the execution of tasks based on specific conditions. This is crucial when you need to execute tasks only when certain criteria are met, making your automation more flexible and dynamic.

**Basic Conditional with `when`**
```bash
- name: Install Nginx only on Ubuntu systems
  apt:
    name: nginx
    state: present
  when: ansible_facts['os_family'] == "Debian"
```
**Variables in Conditionals**
```bash
- name: Install Apache when required
  apt:
    name: apache2
    state: present
  when: install_apache is defined and install_apache == true
```
**Multiple Conditions with `and`, `or`, `not`**
```bash
- name: Install nginx and start it when OS is Ubuntu and version is 20.04
  apt:
    name: nginx
    state: present
  when: ansible_facts['os_family'] == "Debian" and ansible_facts['distribution_version'] == "20.04"
```
```bash
- name: Install nginx on either Ubuntu or CentOS
  apt:
    name: nginx
    state: present
  when: ansible_facts['os_family'] == "Debian" or ansible_facts['os_family'] == "RedHat"
```
```bash
- name: Disable SELinux on CentOS, unless the environment is production
  selinux:
    state: disabled
  when: ansible_facts['os_family'] == "RedHat" and environment != "production"
```
