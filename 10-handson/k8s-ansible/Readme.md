### Kubernetes cluster
To set up a Kubernetes cluster with one master node and two worker nodes using Ansible, you can follow this guide. It will automate the installation and configuration of the Kubernetes control plane (master) and worker nodes.

#### Pre-requisites:
- You have three machines (one master & two worker), either on VMs or Bare Metal.
- The machines have Ubuntu/RedHat.
- SSH access to all the machines.
- Ansible installed on your on a `master node`.

#### Pre-Configuration
- Static Hostname set & check in all machine
```bash
hostnamectl
sudo hostnamectl set-hostname master
sudo systemctl restart systemd-hostnamed
```

Put IP and respective hostname in `/etc/hosts` file of each machine.
```bash
192.168.1.101 master
192.168.1.102 worker1
192.168.1.103 worker2
```
Check ping test each other `ping worker1`, if its not working fix it.
#### Kubernetes Cluster Installation & Configuration through Ansible
##### Step-01
Install Git & Ansible in master node only.
```bash
sudo dnf install git -y
sudo dnf install -y epel-release
sudo dnf install -y ansible
```
If any issue, then
```bash
sudo dnf install -y python3 python3-pip
pip3 install --user ansible
ansible --version
```

#### Steps of K8s Cluster with Ansible
Create an inventory file `inventory.ini` with the master and worker nodes.
```ini
[master]
master ansible_host=192.168.1.101 ansible_user=jakir ansible_ssh_pass=054003 ansible_python_interpreter=/usr/bin/python3

[workers]
worker1 ansible_host=192.168.1.102 ansible_user=jakir ansible_ssh_pass=054003 ansible_python_interpreter=/usr/bin/python3
worker2 ansible_host=192.168.1.103 ansible_user=jakir ansible_ssh_pass=054003 ansible_python_interpreter=/usr/bin/python3

[kubernetes:children]
master
workers
```
Create a variables file `vars.yaml` to store common settings.
```yaml
kubernetes_version: "1.28.0"
pod_network_cidr: "192.168.0.0/16"
calico_manifest_url: "https://docs.projectcalico.org/manifests/calico.yaml"
```
Create an Ansible playbook `k8s-cluster.yaml` that will following steps
- Install dependencies
- Disable swap (Kubernetes requirement)
- Load required kernel modules
- Initialize Kubernetes master
- Join worker nodes
- Deploy Calico CNI

Use SSH Keys Instead of Passwords
```bash
sudo dnf install sshpass -y
ssh-keygen -t rsa -b 4096
ssh-copy-id root@192.168.1.101
ssh-copy-id root@192.168.1.102
ssh-copy-id root@192.168.1.103
```

Lets Ping Test
```bash
ansible all -i inventory.ini -m ping # Ping test
ansible all -i inventory.ini -m ping -vvvv # Ping test with debugging
```

Lets Run the Playbook
```bash
ansible-playbook -i inventory.ini site.yaml
ansible-playbook -i inventory.ini site.yaml -vvvv # In debugging
```
