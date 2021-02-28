## Introduction to Ansible Playbook
### Some limitations of ad-hoc commands:
* fails the Infrastructure-as-a-Code paradigm.
* plays can't be source controlled.
* limit the reusability. 

#### Playbook to the rescue!!

* offers reusability, simple configuration management and multi-machine deployment.
* can be source controlled.
* orchestrate steps of any manual ordered process, on multiple hosts/groups, in a defined ordered.
* expressed in YAML format with minimum of syntax.
* composed of one or more 'plays' in an ordered list. 

To draw an analogy between ansible and cooking: 'play' is the steps in cooking/baking process and 'playbook' is the cookbook and 'modules' are the ingredients.

At minimum, each play defines two things:
1. the managed nodes to target, using a pattern.
2. at least one task to execute.
 
Example: Install Apache HTTP server [1] on managed hosts
```
---
- name: Install Apache
  hosts: webservers
  remote_user: root

  tasks:
  - name: ensure apache is at the latest version
    apt:
      name: apache2
      state: latest

  - name: write the apache config file
    service:
      name: apache2
      state: started
```

Notes:
* Playbook with multiple 'plays' can orchestrate multi-hosts actions. 
* Ansible executes each task in order, one at a time, against all machine matched by the host pattern. If a task fails on a host, Ansible takes that host out of the rotation for the rest of the playbook.
* Each task executes a module with specific arguments. 
* Idempotency: most of the ansible modules checks if the desired state is achieved and takes no action once achieved. Hence, repeating the tasks doesn't change the final state. 
* Use _--verbose_ flag when running the playbook to see detailed output. 

### Using variables in Ansible [2]
* Variables are required to manage difference between systems, stages, environment etc. 
* Variables can be created with standard YAML syntax, including lists and dictionaries. 
* Variables can be defined in the inventory, in re-usable files, roles, at the command line or during the playbook run by registering the return value.
* Understanding the variable precedence is of great importance in real-world scenarios. 
* refer to vars_sample.yml playbook

### Templating (Jinja2) in Ansible [3]

* Ansible uses Jinja2 templating to enable dynamic expressions and access to variables. 
* Ansible includes many specialized filters and tests for templating.
* All templating happens on the Ansible controller before the task is sent and executed on the target machine. Hence, Jinja2 is only required on the controller. 

### Loops in Ansible [4]
* to repeat a taks multiple times. 
* refer to loop_sample.yml playbook

### Conditionals in Ansible [5]
* the result of play may depend on the value of a variable, remote system facts or previous tasks results. 
* refer to install_apache_conditional.yml

### Blocks in Ansible [6]
* Blocks are used in two different scenarios:
1. For grouping tasks: All tasks in a block inherit directives applied at the block level. 
2. For handling error: similar to other programming languages, uses block with _rescue_ and _always_ section
* refer to install_apache_blocks.yml

### Handlers in Ansible [7]
* similar to a normal task but they run if the task contains a notify directive. 
* it runs only once, after all the tasks are completed in a play, irrespective of how many tasks notify a handler.
* refer to install_apache_handlers.yml

#### References: 
1. Apache HTTP server [link](https://httpd.apache.org/)
2. Using Variables [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_variables.html)
3. Templating (Jinja2) [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_templating.html)
4. Loops [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_loops.html)
5. Conditionals [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_conditionals.html)
6. Blocks [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_blocks.html)
7. Handlers [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_handlers.html)