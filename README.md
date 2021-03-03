# ansible-provisioning-example
Provisioning the deployment environment using Ansible, to install Apache, JDK, Postgresql and Docker. 

Tags: Ansible, Devops

# Ansible Controller

1- Creating a working directory

```bash
mkdir ~/ansible-provisioning-example
```

2- Creating the [inventory file](inventory.yml)

3- Creating the variables file

```bash
mkdir ~/ansible-provisioning-example/group_vars
echo "---" > ~/ansible-provisioning-example/group_vars/all.yml
```

3- Creating a [passphrase file](.ansible-vault-pw) for ansible vault

```bash
echo "myvaultpassword" > ~/ansible-provisioning-example/.ansible-vault-pw
```

4- Creating an encrypted variable for the remote host password and appending it to the [variables file](group_vars/all.yml)

```bash
ansible-vault encrypt_string \
	--vault-id deploy@~/ansible-provisioning-example/.ansible-vault-pw \
	'your_remote_host_password_here' \
	--name 'vault_deploy_server1_password' \
	>> ~/ansible-provisioning-example/group_vars/all.yml
```

5- Creating a [configuration file](ansible.cfg) to disable the Host Key Checking

6- Testing the ping between the Ansible Controller server and the Deployment Server

```bash
ansible deploy_servers -m ping -i inventory.yml --vault-id deploy@.ansible-vault-pw
```

> server1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}

7- Writing the [playbook](playbook.yml) for provisionning the following environment :

- Apache 2.4
- JDK 11
- Postgres DB
- Docker

8- Executing the playbook :

```bash
ansible-playbook -i inventory.yml --vault-id deploy@.ansible-vault-pw playbook.yml
```
