## Wildfly v13 (earlier Jboss AS) deployment in Standalone mode

**Pre-requisite**
- Requires Ansible 2.9 or newer
- Expects Ubuntu 18.04 or newer
- Managed hosts are setup correctly

### Notes

* This playbook deploy a simple implementation of Wildfly v13 (earlier known as JBoss AS), a community version of JBoss application server offering.
* includes two roles:
	1. ```install-jboss-standalone``` : install Wildfly/JBoss on managed node. 
	2. ```deploy-java-app``` : deploy a simple java application on the running Wildfly instance.
* includes group_vars to set the common variables for host group.
	* ```group_vars/all.yml``` file to set common variables for Wildfly/JBoss.

### Run the playbook:
```
	ansible-playbook -i servers roles-example/wildfly-run.yml
```
When the playbook run completes, we should be able to see the Wildfly server running on the ports we defined, on the target machines. The sample Java application deployed on the server and accessible over http://localhost:8080

Note: the ports for wildfly server running as docker container are not exposed, and can be accessible by ssh to container or with ansible modules like shell (using curl)

**Verify**
```
ansible servers -m shell -a 'curl http://localhost:8080'
```
check on selected host:
```
ansible servers -m shell -a 'curl http://localhost:8080' -l <host-name>
```
