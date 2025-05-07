## Summary of What the Playbook Does
The playbook automates the following steps to set up and configure an Apache web server on a RedHat-based system:  
- Install Apache and start the service.  
- Modify the Apache configuration to change the listening port from 80 to 88.  
- Backup existing configurations and restore them if needed.  
- Ensure Apache can serve content by uploading an index.html page.  
- Adjust firewall and SELinux settings to allow traffic on the new port.  
- Restart Apache to apply the changes.  

### Create the ansible.cfg

The ansible.cfg file is the configuration file for Ansible, where you can set default options to customize how Ansible runs. In this example, it has two settings:  
**inventory = ~/inventory**: This tells Ansible where to find the inventory file, which lists the servers (or hosts) you want to manage. In this case, it's located in the user's home directory (~) and is named inventory.  
**remote_user = root**: This specifies that Ansible will connect to the remote machines using the root user by default. This is the user that Ansible will use to perform tasks on the target servers.  
These settings help Ansible know where to find the inventory file and how to connect to the servers without needing to specify this information every time you run a playbook.  

---

### Create the inventory file

[ws]: this section defines a group called ws, which contains a single host: workstation.  
[webservers]: this section defines a group called webservers with two hosts: servera and serverb.  
[dbservers]: this section defines a group called dbservers with two hosts: serverc and serverd.  
[nti:children]: the nti group is a parent group, and it includes the child groups webservers and dbservers. This means that nti will automatically include all the hosts from the webservers and dbservers groups (i.e., servera, serverb, serverc, and serverd).  

---

### Create the vault which will have the variables

An **Ansible Vault** is a tool provided by Ansible to securely store sensitive data, such as passwords, API keys, or other confidential information, in an encrypted file. The use of Ansible Vault allows you to keep sensitive variables safe while still allowing automation tasks to reference them within your playbooks.  

**Steps and Explanation:**  
- Creating the Vault File:

```
ansible-vault create vars.yml
```
This command is used to create a new vault file named vars.yml. This vault will contain sensitive variables that you don't want to expose in plain text.When you run this command, it will prompt you to enter a password. This password will be required to access and edit the contents of the vault in the future.  
- Defining Variables in the Vault:
Once the vault is created, you'll add variables to it. In this case, two variables are defined:
```
httpd_conf: /etc/httpd/conf/httpd.conf
backup_dir: /home/student/backupdir
```
**httpd_conf**: This variable defines the location of the Apache web server’s configuration file (httpd.conf). This is used in the playbook to reference the Apache configuration file for tasks such as modifying or backing it up.  
**backup_dir**: This variable defines the directory where the Apache configuration file should be backed up. It specifies that the backup will be stored in /home/student/backupdir.  

---

### Create the file that will have the password of the vault file vars
When you create a vault to securely store sensitive variables (such as the vars.yml vault), you will need to provide a password to decrypt it when running your playbooks. This password is typically stored in a separate file for ease of use, especially in automation scenarios.  

**Steps and Explanation**:

- Creating the Password File:  
This command is used to create a file that contains the password for the vault, it writes the password nti to a file named mypass. The password nti will be used to decrypt the vault file (vars.yml) when the playbook is executed.
```
echo "nti" > mypass
```
Why use a password file: Storing the password in a file makes it easier to automate the process of running playbooks that require vault access. Instead of entering the password manually each time, the file can be referenced during the playbook execution.  
To ensure the security of the password file, it is important to restrict access to it. This is done by running the following command:  
```
chmod 600 mypass
```

The chmod 600 command sets the file permissions so that only the file owner (you) can read and write the mypass file. No one else on the system will be able to access this file.

---

### Finally check and run the playbook
Make sure to place all these files in your **home** to let the playbook run.  

```
ansible-navigator run -m stdout myplaybook.yml --pae=false --vault-password-file=mypass --check
```

This command runs an Ansible playbook (myplaybook.yml) using Ansible Navigator, outputting the results to the terminal (stdout mode). It disables the Playbook Analysis Engine (--pae=false), uses the password file (--vault-password-file=mypass) to decrypt encrypted variables in the vault, and runs the playbook in dry-run mode (--check) to simulate the changes without actually applying them.  

```
ansible-navigator run -m stdout myplaybook.yml --pae=false --vault-password-file=mypass
```
This command runs an Ansible playbook (myplaybook.yml) using Ansible Navigator, which provides an interactive experience and outputs the results to the terminal (stdout). It disables the Playbook Analysis Engine (--pae=false), and uses a password file (--vault-password-file=mypass) to decrypt any vault-encrypted variables during execution. The playbook will be run normally, and the results will be shown in real-time in the terminal window.