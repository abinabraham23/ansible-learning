## Encryption with Ansible Vault
* Ansible vault encrypts variables and files to protect sensitive contents like password, keys etc. 
* Need one or more passwords to encrypt and decypt the contents, used with **_ansible-vault_** CLI. 
* Can create/view encrypted variables, create encrypted files, encrypt existing files, edit/re-key/decrypt files. 
* Encrypted variables can be then stored in SCM and shared with teams.
* Use encrypted variables and files in ad-hoc commands and playbooks, by supplying the passwords used to encrypt them.

### Create encrypted variables
```
ansible-vault encrypt_string <password_source> '<string_to_encrypt>' --name '<string_name_of_variable>'
```
for example:
```
echo -n 'vault_content' | ansible-vault encrypt_string --vault-password-file vault_pass --stdin-name 'secret'
```
or to pass it interactively via prompt:
```
ansible-vault encrypt_string --vault-password-file vault_pass --stdin-name 'secret'
```
The output from above command i.e. encrypted variables can be added to playbook, variable files or role for use. The decryption will happen on the playbook runtime, depending on the vault password provided. 

_Note_: In case don't want to store the vault password in a file, then it's also possible to provide the vault password with each command run by passing **_--ask-vault-pass_** argument. For example:
```
ansible-vault encrypt_string --ask-vault-pass --stdin-name 'secret'
```

For next step, store the encrypted variable (example below) in a file named vault.yml 
```
secret: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    39636532323963323161386432636665303031396635353437333133303866386664336334626563
    3237333363356530363862633264623636613662613666660a303465386633646566353832353631
    35373036366437336332666631663433353061393564666332616231343738643537326264386566
    3935356238623662610a396261373461363138376536393132643365633232383532363564393565
    6630
```

### View encrypted variables
Encrypted content can be viewed using _debug_ module with vault password
```
ansible localhost -m debug -a var="secret" -e "@vault.yml" --vault-password-file vault_pass
```

### Create encrypted files
Let's create a new encrypted data file called ‘foo.yml’ with the vault password from *vault_pass* file:
```
ansible-vault create --vault-password-file vault_pass foo.yml
```
Note: the tool launches an editor to enter the sensitive data, default is **vi**. This can be overridden with $EDITOR (export EDITOR=nano). 

### Encrypt existing files
Let's create multiple yml files with random variable(s), ex: var1.yml, var2.yml
```
ansible-vault encrypt --vault-password-file vault_pass var1.yml var2.yml
```
### View encrypted files
```
ansible-vault view --vault-password-file vault_pass var1.yml var2.yml
```
### Edit encrypted files
```
ansible-vault edit --vault-password-file vault_pass var1.yml
```
### Changing the password of encrypted files
```
ansible-vault rekey --vault-password-file vault_pass var1.yml
```
### Decrypt encrypted files
```
ansible-vault decrypt --vault-password-file vault_pass var1.yml
```

### References:
1. Encrypting content with Ansible Vault. [link](https://docs.ansible.com/ansible/2.10/user_guide/vault.html)