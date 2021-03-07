# Installing Apache on Ubuntu
Apache HTTP server is the most widely-used web server in the world. 
Provides many powerful features including dynamically loadable modules, robust media support, and extensive integration with other popular software.

### Pre-requisite
1. Ansible Control Node with Ansible installed and configured to connect to Ansible host using SSH keys. Control node to have a regular user with sudo permissions and firewall enabled.
2. Ansible Hosts should be up and running with Ubuntu 18.04 installed.

### Directory structure and component details
```
real-world-example
├── files
│   ├── apache.conf.j2
│   └── index.html.j2
├── vars
│   └── default.yml
├── install-apache.yml
└── readme.md
```

1. **files/apache.conf.j2**: Template file for setting up Apache Virtual Host.
2. **files/index.html.j2**: Template file for setting up the default page on web server’s root directory.
3. **vars/default.yml**: variable file for customizing playbook settings.
4. **install-apache.yml**:  playbook file, containing the tasks to be executed on the remote server(s).

## Playbook steps
1. Create group and add user to the group.
2. Install Apache and firewalld with *apt* module ad start the services.
3. Create a custom document root directory for the new Apache VirtualHost and set up a test page.
4. Setup and enable the new Apache VirtualHost.
5. Disable the default Apache web page when the disable_default flag is set to true.
6. Open default port# 80 to access the static page and trigger restart for firewalld service. 

Once the playbook has finished running, we will have a web server running on our target node, based on the options we defined within the configuration variables.

### Verify the static page on running Apache
```
ansible servers -m shell -a 'curl http://localhost:80'
```
or 
```
ansible servers -m uri -a "url=http://localhost:80 return_content=yes"
```