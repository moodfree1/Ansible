# dr1-ansible-dev

# Ansible Playbooks:

- ./site.yml is the master playbook.
- ./site.yml calls the ./“.yml” playbook specified in ./site.yml.
- Simply update ./site.yml, the imported ./”.yml” from site.yml, the ./group_vars/”.yml” for the specific task, and the inventory file.
- The imported ./“.yml” is the file where you would add your specific roles/tasks.

For example, to add users to the informatica servers:
- Simply create an inventory file and include the hosts where you want to add users.
- Next, update ./site.yml to import the ./informatica.yml playbook.
- Update ./informatica.yml to include the UserAdd role.
- Now, update the variables in ./group_vars/UserAdd.yml. This would include the usernames you want to add to the hosts from your inventory file.

Then, run this command:

>ansible-playbook -i <path_to_inventory_file> site.yml --private-key <path_to_pemkey> -vvv

To limit to one server in your inventory use the --limit flag:

>ansible-playbook i <path_to_inventory_file> site.yml --private-key <path_to_pemkey> -limit sandbox-test –vvv

To run a syntax check first, use the --syntax-check flag:

>ansible-playbook i <path_to_inventory_file> site.yml --private-key <path_to_pemkey> --syntax-check

As we write playbooks, it's important to keep them lean (micro services).  In this way, we can reuse roles.
For example, one could include several roles or micro services in informatica.yml:

- GrpAdd.yml
- UserAdd.yml
- some_role_Jane_wrote_that_does_something
- some_role_Joe_wrote_that_does_something_else

The ansible configuration file (/etc/ansible/ansible.cfg) allows one to set defaults for ansible flags.

As with any script or configuration management system, one must always test new code before making any bulk changes! Ansible has several flags for performing a dry run (no operation) command to verify code functionality. Examples include:

- ansible-playbook site.yml –i <path_to_inventory_file> --check
- ansible-playbook site.yml –i <path_to_inventory_file> --list-hosts
- ansible-playbook site.yml –i <path_to_inventory_file> --list-tasks


# Ansible Directory Structure:

#Inventory files:

- hosts/DB2
- hosts/Informatica
- hosts/hosts

#Assign variables to particular groups:

group_vars/
- common.yml
- ChgPerms.yml
- CmdRoot.yml
- CmdUser.yml
- CpFiles.yml
- GrpAdd.yml
- GrpDel.yml
- MkDir.yml
- SrvEnable.yml
- Sudoers.yml
- SWInstall.yml
- UserAdd.yml
- UserDel.yml
- vault.yml (encrypted UserAdd default password)

#Assign variables to particular systems:

host_vars/
- hostname1.yml
- hostname2.yml

vars/
- default_vars

#Custom modules (optional):

- library/
- module_utils/
- filter_plugins/

#Playbooks:

- site.yml (master playbook)
- informatica.yml
- db2.yml

#Files (e.g.; CpFile):

files/
- .profileDB2
- authorized_keys
- cronDB2
- dbaDB2.conf
- resolv.conf
- ulimits

#This hierarchy represents a "role". The playbooks in the gitlab ansible repo are broken into "micro services", separated by roles:

roles/
- common/defaults/
-        main.yml
- common/handlers/
-        main.yml
- common/meta/
-        main.yml
- common/README.md
- common/tasks/
-        main.yml
- common/tests/
-        main.yml
- common/vars/
-        main.yml

#Same kind of structure as "common" role (excluding unneeded dir/files):

- ChgPerms/tasks/
-        main.yml

- CmdRoot/tasks/
-        main.yml

- CmdUser/tasks/
-        main.yml

- CpFiles/tasks/
-        main.yml

- GrpAdd/tasks/
-        main.yml

- GrpDel/tasks/
-        main.yml

- MkDir/tasks/
-        main.yml

- ReachableHosts/tasks/
-        main.yml

- SrvEnable/tasks/
-        main.yml

- Sudoers/tasks/
-        main.yml

- SWInstall/tasks/
-        main.yml

- UserAdd/tasks/
-        main.yml

- UserDel/tasks/
-        main.yml
