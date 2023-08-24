# Inventory file
- Located by default at `/etc/ansible/inventory`
- Most common format are `INI` or `YAML`
- This lists all the machine available
- To use a different inventory file, use `-i <path/to/file>` on the command line

Sample inventory file with a `txt` format. 
``` 
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!


[web_servers]
web1
web2
web3

[db_servers]
db1

[all_servers:children]
web_servers
db_servers

```

> For Linux, `ansible_connection=ssh`, for Windows `ansible_connection=winrm`
> For Linux, use `ansible_ssh_pass`. For Windows use `ansible_password`

# Playbook
- Always written in `YAML` format
-  Defines a set of activities (tasks) to be run on hosts
	- Task:
		- Execute a command
		- Run a script
		- Install a package
		- Shutdown/Restart
- `ansible-playbook` to run a playbook. Different than `ansible` which runs a one off

```yaml
- name: 'Stop the web services on web server nodes'
. hosts: web_nodes
. tasks:
	- name: 'Stop the web services on web server nodes'
      command: service httpd stop
- name: 'Shutdown the database services on db server nodes'
  hosts: db_nodes
  tasks:
    - name: 'Shutdown the database services on db server nodes'
      command: service mysql stop
- name: 'Restart all servers (web and db) at once'
  hosts: all_nodes
  tasks:
    - name: 'Restart all servers (web and db) at once'
      command: /sbin/shutdown -r
- name: 'Start the database services on db server nodes'
  hosts: db_nodes
  tasks:
    - name: 'Start the database services on db server nodes'
      command: service mysql start
- name: 'Start the web services on web server nodes'
  hosts: web_nodes
  tasks:
    - name: 'Start the web services on web server nodes'
      command: service httpd start
```


# Modules
Modules take care of some overhead required to perform an operation. Should be enough for most usecase. Just find the right module.
[link](https://docs.ansible.com/ansible/2.3/modules_by_category.html)
- Different type of modules operating at different levels
	- System
		- User
		- Group
		- Hostname
		- Iptables
		- Ping
		- Service
		- Systemd
		- ...
	- Commands
		- Command
		- Expect
		- Raw
		- Script
		- Shell
		- ...
	- Files
		- Acl
		- Archive
		- Copy
		- File
		- Find
		- Stat
		- ...
	- Database
		- Mongdb
		- MSSQL
		- MySQL
		- ...
	- Cloud
		- Amazon
		- Azure
		- Docker
		- Google
		- ...
	- More...

Different modules in a playbook
```yaml
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Update entry into /etc/resolv.conf'
            user:
                name: web_user
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present

```
