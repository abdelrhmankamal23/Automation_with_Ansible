## Building Role in Ansible for an Apache Web Server
This is the role for Task_1 so lets start with  

```
mkdir roles
cd ./roles
ansible-galaxy init apache_role
```
The command creates a standard role **directory structure**:  
`ansible-galaxy` is the main command-line tool for working with Ansible roles and collections. It allows you to create new role structures (init), download and share roles with the Ansible Galaxy community, and manage collections and dependencies.  
`init` is the subcommand tells Ansible to initialize (create) a new role structure.  
`apache_role` is the name of the role you are creating. It will create a directory called apache_role with all the standard subdirectories and files needed for an Ansible role.  

```
tree
```
will show the following
```
apache_role/
├── defaults/
│   └── main.yml         # Default variables
├── files/               # Static files to copy to the target
├── handlers/
│   └── main.yml         # Handler tasks (e.g., restarting services)
├── meta/
│   └── main.yml         # Role metadata (e.g., dependencies)
├── README.md            # Documentation for the role
├── tasks/
│   └── main.yml         # Main list of tasks
├── templates/           # Jinja2 templates
├── tests/
│   └── inventory       # to verify role behavior without affecting production systems.
│   └── test.yml        # verifies the role produces the expected system state
├── vars/
│   └── main.yml         # Other variables
```
Then  

```
ansible-navigator run -m stdout myplaybook.yml --vvv
```
The --vvv flag enables very detailed (verbose) logging, useful for debugging, as it shows exactly what Ansible is doing behind the scenes at each step.  
Now, run the playbook and enjoy the automation.  