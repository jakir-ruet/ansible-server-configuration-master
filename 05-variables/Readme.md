In **Ansible**, variables are a powerful and flexible way to customize playbooks, roles, and tasks. They allow you to dynamically change configurations or values depending on the environment or conditions, making your automation more reusable and flexible.

**Types of Variables**
1. Host Variables
```bash
inventory/
  host_vars/
    web01.example.com.yml
    db01.example.com.yml
```

2. Group Variables
```bash
inventory/
  group_vars/
    webservers.yml
    dbservers.yml

```

3. Global Variables
```bash
[defaults]
inventory = ./inventory
```

**Variable Precedence** Ansible variables have a specific precedence order, which determines which value will be used when there are conflicting definitions. Higher precedence variables override lower precedence ones. Variable Precedence (from lowest to highest):
- Default Variables (e.g., in roles)
- Inventory file (host and group vars)
- Playbook Variables
- Command-line Variables (-e or --extra-vars)
- Facts (gathered during playbook execution)
- Environment Variables (set in the environment)
