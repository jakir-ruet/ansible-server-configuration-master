**Types of Inventory in Ansible:**
- **Static Inventory:** A static inventory is a simple text file that lists the hosts you want to manage. The file can be in `INI`-style format or `YAML` format.

- **Dynamic Inventory:** A dynamic inventory is generated dynamically by pulling data from external sources like cloud providers (AWS, GCP, Azure), CMDBs, or a custom database. This allows Ansible to discover hosts automatically and eliminates the need to manually maintain an inventory file.
- **Inventory via Ansible’s -i Flag:** When running a playbook, you can specify a custom inventory using the `-i` option. This allows you to run a playbook targeting a specific set of hosts.
