# Ansible Role
### Why do we need it? 
* writing ansible code to manage the same service for multiple environments or different products increase code redundancy.
* with more complexity in functionality, it becomes difficult to manage everything in one ansible playbook file. 
* Ansible Role helps solve these problems.

### Solution

* Ansible role is an independent component which allows reuse of common configuration steps, and will be used withing the playbook. 
* Ansible role is a set of tasks to configure the host(s), to serve a certain purpose.
* Ansible role(s) in use needs to be defined in following two locations:
    * directory *roles/* relative to playbook file.
    * under */etc/ansible/roles*.
    * alternatively, set the ```roles_path``` in ansible.cfg file or environment variable ```ANSIBLE_ROLES_PATH```
* An Ansible role has a defined directory structure and one below for reference:
```
roles/
    common/
        tasks/
        handlers/
        library/
        files/
        templates/
        vars/
        defaults/
        meta/
    webservers/
        tasks/
        defaults/
        meta/
```
* __tasks__: contains the main steps to be executed by the role.
* __handlers__: contains handlers which can be invoked by “notify” directives and are associated with service.
* __library__: used to embed modules and plugins in roles.
* __files__: contains files which we can be copied to the remote host.
* __templates__: contains file template which supports modifications from the role. We use the Jinja2 templating language for creating templates.
* __vars__: contains variables for the role. Variables in vars have higher priority than variables in defaults directory.
* __defaults__: contains default variables for the role. Variables in default have lowest priority and can be easily overridden.
* __meta__: contains metadata of role like an author, support platforms, dependencies.

__Notes:__ 
* each directory must contain a *main.yml* file which contains relevant content.
* other than *main.yml*, we can add addional YAML files and reference it in the *main.yml*. For example, a platform specific tasks in seperate file and can be refered in tasks/main.yml using *import_tasks* directives.

### Using Role in a playbook
Ansible Role can be used in three ways:
1. at play level with ```roles``` option. 
2. at task level with ```include_role```, can be reused dynamically anywhere in the ```tasks``` section of play. 
3. at task level with ```import_role```, can be reused statically anywhere in the ```tasks``` section of play. 

### References
1. Roles - Ansible documentation [link](https://docs.ansible.com/ansible/2.10/user_guide/playbooks_reuse_roles.html#roles)