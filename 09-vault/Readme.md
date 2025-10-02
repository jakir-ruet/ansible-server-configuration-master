### Vault

A Vault is a feature that lets you `encrypt sensitive` data (like passwords, private keys, API tokens, etc.) so they can be safely stored in playbooks, roles, or variable files without exposing secrets in plain text.

#### Key Points about Ansible Vault

- Encrypts YAML files or even individual variables.
- Uses AES256 encryption by default.
- Requires a password or key file to decrypt and use secrets during playbook execution.
- Keeps your sensitive information safe in version control (Git, etc.).

#### Common vault commands

|Example                                                           | Description                                    |
|----------------------------------------------------------------- | ---------------------------------------------- |
|`ansible-vault create secrets.yaml`                                | Create a new encrypted file                    |
|`ansible-vault encrypt vars.yaml`                                  | Encrypt an existing file                       |
|`ansible-vault decrypt secrets.yaml`                               | Decrypt an encrypted file back to plain text   |
|`ansible-vault edit secrets.yaml`                                  | Open and edit an encrypted file in editor      |
|`ansible-vault view secrets.yaml`                                  | View contents of an encrypted file (read-only) |
|`ansible-vault rekey secrets.yaml`                                 | Change the vault password for a file           |
|`ansible-playbook site.yaml --ask-vault-pass`                      | Run a playbook with vault password prompt      |
|`ansible-playbook site.yaml --vault-password-file .vault_pass.txt` | Run a playbook using a password file           |

#### Let's explore anSecuring DB Password with Vault

- Create an encrypted secrets file

```bash
ansible-vault create secrets.yaml
Ans@054003
db_password: Ans@054003
:wq # save & exit
```

- Should show

```bash
$ANSIBLE_VAULT;1.1;AES256
62306235353363356636326239376533373536383439363863306334616562623136323462393361
6538323234656530386164313566393662333230316239650a663435353563323131643061396266
34633430343632633635643439313832333932313562306637623131373566306164396264613234
6366646664313639390a376234336234623731343238396537333035303461346665306532663661
31393966393335356563326638313338333130373865363430613336343765656530
```

- Create the playbook name `db.yaml`

```bash
sudo vi db.yaml
```

```bash
- hosts: dbserver
  become: yes
  vars_files:
    - secrets.yaml
  tasks:
    - name: Ensure MySQL user exists with vault password
      mysql_user:
        name: myuser
        password: "{{ db_password }}"
        priv: "*.*:ALL"
        state: present
```

```bash
ansible-playbook db.yaml --ask-vault-pass
Ans@054003
```
