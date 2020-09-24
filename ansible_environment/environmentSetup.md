### To Do: Create a ansible playbook to create docker containers as ansible playground

### Link: https://medium.com/@dcarrascal75/a-simple-ansible-playground-using-docker-eeb458cbba32
### Github Link: https://github.com/dcarrascal75/ansible-playground.git 

# Ansible playground using Docker 

* Create an SSH key using `ssh-keygen`

Note: This ssh key will allow us to connect to docker containers using ssh directly. The above command will generate a pair of files: public key (_id_rsa.pub_) and private key (_id_rsa_). 

* Prepare Dockerfile to build a docker image with ssh enabled and python3. In addition, we need to include our new key in the authorized_keys file of the root user. Refer to the Dockerfile

* Build the image based on the Dockerfle with 
`docker build -t ubuntu-ssh .`

* Run the first container as host1/server1 with specified port and hostname in ansible hosts
`docker run --name server1 --hostname server1.test -p 2224:22 -d ubuntu-ssh`

* Test with SSH to the newly created container with private key as input and root user
  `ssh -i 'id_rsa' -p 2224 root@localhost`
 
 * Run the second container as host2/server2 with specified port and hostname in ansible hosts
`docker run --name server2 --hostname server2.test -p 2225:22 -d ubuntu-ssh`

* Test with SSH to the newly created container with private key as input and root user
 `ssh -i 'id_rsa' -p 2225 root@localhost`

 __Note__: In case of any error with known-hosts, run the below command to remove the known host:
`ssh-keygen -f "~/.ssh/known_hosts" -R "[localhost]:2224"`

## Ansible setup

* set ssh_connection parameter in ansible.cfg file (under /etc/ansible/)
[ssh_connection]
transport=ssh

* Create a ssh config file as ~/.ssh/config to disable the host key checking and with IdentityFile, specify which Private Key to use. In addition, we define our hosts which corresponds with a Docker container, that resides within our local machine and it is mapped by its exposed port. (here 2224 & 2225).

* Create an inventory file (in this case under ~/.ansible/inventory) with below server hostname under group "servers"
[servers]
server1.test
server2.test

* Test the connectivity (inventory file are read runtime instead of static file):
`ansible all -i '~/.ansible/inventory' -m ping`

## Start / Stop Docker 

* Start Docker Daemon
`sudo /etc/init.d/docker start`

* Start Docker Daemon
`sudo /etc/init.d/docker stop`
