# Ansible playground using Docker 

* Create an SSH key using `ssh-keygen`

ssh-keygen -b 2048 -t rsa -f sshkey -q -N ""

Note: This ssh key will allow us to connect to docker containers using ssh directly. The above command will generate a pair of files: public key (_sshkey.pub_) and private key (_sshkey_).

* Prepare Dockerfile to build a docker image with ssh enabled and python3 installed. In addition, we need to include our new key in the authorized_keys file of the root user. Refer to the Dockerfile

* Build the image based on the Dockerfle with 
`docker build -t ansible-host .`

* Run the first container as host1/server1 with specified port and hostname in ansible hosts
`docker run --name host1 --hostname host1.test -p 7771:22 -d ansible-host`

* Test with SSH to the newly created container with private key as input and root user
  `ssh -i sshkey -p 7771 root@localhost`
 
 * Run the second container as host2/server2 with specified port and hostname in ansible hosts
`docker run --name host2 --hostname host2.test -p 7772:22 -d ansible-host`

* Test with SSH to the newly created container with private key as input and root user
 `ssh -i sshkey -p 7772 root@localhost`

 __Note__: In case of any error with known-hosts, run the below command to remove the known host:
`ssh-keygen -f "~/.ssh/known_hosts" -R "[localhost]:7771"`

## Ansible setup

* Create an 'inventory' file with below hostnames under group "servers"
[servers]
host1.test
host2.test

* Create a ssh config file (ssh.config) to define our remote hosts which corresponds with running Docker containers, that resides within our local machine and it is mapped by its exposed port. (here 7771 & 7772). In addition, we disable the host key checking and with IdentityFile, specify which Private Key to use. 

Host *
    #disable host key checking: avoid asking for the keyprint authenticity
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    #enable hashing known_host file
    HashKnownHosts yes
    #IdentityFile allows to specify exactly which private key I wish to use for authentification
    IdentityFile sshkey

Host host1.test
    HostName localhost
    User root
    Port 7771

Host host2.test
    HostName localhost
    User root
    Port 7772

* Set custom inventory and ssh_connection parameter in ansible.cfg file
[defaults]
inventory = inventory

[ssh_connection]
transport=ssh
ssh_args= -F ssh.config

* Test the connectivity (inventory file are read runtime instead of static file):
`ansible servers -i inventory -m ping`

## Start / Stop Docker 

* Start Docker Daemon
`service docker start`

* Start Docker Daemon
`service docker start`

### To Do: Create a ansible playbook to create docker containers as ansible playground