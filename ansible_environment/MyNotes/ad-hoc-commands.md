# Ansible Ad-hoc commands
* uses /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes.
* quick and easy, but not reusable.
* helps to understand how Ansible works.

ansible [pattern] -m modules -a ["module options"]

**Patterns** let you run commands and playbooks against specific hosts and/or groups in your inventory. An Ansible pattern can refer to a single host, an IP address, an inventory group, a set of groups, or all hosts in your inventory.

**Modules** are reusable, standalone scripts that can be used by the Ansible API, or by the ansible or ansible-playbook programs.

Note: 
* default module for ansible CLI is ansible.builtin.command module
* before running adhoc commands, the servers should be listed in inventory and working SSH credentials for each machine in that group. 
* before execution of ansible commands (via ad-hoc or playbook), we must chose which managed nodes or groups we want to execute against.


Use-cases:
* Test the reachability of managed host(s) using _ping_ module
```
ansible servers -m ping

ansible servers -m ping -f 2 
```
Note: -f or --fork specify number of parallel processes to use (default=5)

* Gather facts about the managed system(s)/node(s) using _setup_ module
```
ansible servers -m setup
Using filter:
ansible servers -m setup -a 'filter=ansible_distribution*'
```

* Rebooting remote servers (uses default _command_ module)
```
ansible servers -a "/sbin/reboot"

ansible servers -a "/sbin/reboot" -u operator --become [--ask-become-pass]
```
Note: **become** keyword is used for privilege escalation. In above command, we can connect to server as operator and run the command as root user. When we add --ask-become-pass or -K, ansible prompts for the password to use for privilege escalation(sudo, su, doas etc..)

* Using different module than _command_ (default), like _shell_ module
```
ansible servers -m shell -a 'echo $HOSTNAME'
```
Note: pay particular attention to shell quoting rules, so the local shell retains the variable and passes it to Ansible. For example, using double rather than single quotes in the above example would evaluate the variable on the box you were on.

* Transfer files to multiple machine in parallel using _copy_ module. 
```
ansible servers -m copy -a "src=/etc/hosts dest=/tmp/hosts"

to verify: 
ansible servers -m shell -a 'cat /tmp/hosts' 
```
* Create a user on managed hosts using _user_ module
```
ansible servers -m user -a "name=operator password=test"
```
* Change ownership and permissions on files using _file_ module.
```
ansible servers -m file -a "dest=/tmp/hosts mode=600 owner=operator group=operator"

to create a directory:
ansible servers -m file -a "dest=/tmp/testdir mode=755 owner=operator group=operator state=directory"
```
* Manage packages (install, update or remove) using package management modules like _yum_ , _apt_ or _package_ (generic)
```
Check package status
ansible servers -m shell -a 'apt list apache2'

Install:
ansible servers -m apt -a "name=apache2 state=present"
Update:
ansible servers -m apt -a "name=apache2 state=latest"
Remove:
ansible servers -m apt -a "name=apache2 state=absent"
```

* Manage services using _service_ module
```
Check service status:
ansible servers -m shell -a 'service apache2 status'

Start:
ansible servers -m service -a "name=apache2 state=started"
Restart:
ansible servers -m service -a "name=apache2 state=restarted"
Stop:
ansible servers -m service -a "name=apache2 state=stopped"
```
