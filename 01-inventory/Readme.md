### Inventory

An inventory in Ansible is a source of truth that defines what machines you are managing, and optionally, how to connect to them. It can be:

- A `static`file (INI, YAML) listing hosts and groups.
- A `dynamic` source (cloud plugin - `AWS`, `GCP`, `Azure` external script, CMDB, API) that fetches hosts automatically.

#### Static Inventory (YAML/INI file) - `inventory.yaml`

```bash
all:
  hosts:
    web1:
      ansible_host: 192.168.1.10
      ansible_user: ubuntu
    web2:
      ansible_host: 192.168.1.11
      ansible_user: ubuntu
  children:
    db:
      hosts:
        db1:
          ansible_host: 192.168.1.20
          ansible_user: ubuntu
```

```bash
ansible -i inventory.yml all -m ping
```

#### Dynamic Inventory (Script or Plugin) - `inventory.py`

```bash
#!/usr/bin/env python3
import json

inventory = {
    "all": {
        "hosts": ["web1", "web2"],
        "vars": {
            "ansible_user": "ubuntu"
        }
    },
    "_meta": {
        "hostvars": {
            "web1": {"ansible_host": "192.168.1.10"},
            "web2": {"ansible_host": "192.168.1.11"}
        }
    }
}
print(json.dumps(inventory))
```

```bash
chmod +x inventory.py # give privilege
```

```bash
ansible-inventory -i inventory.py --list # check
ansible -i inventory.py all -m ping # run
```
