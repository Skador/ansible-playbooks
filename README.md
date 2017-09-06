# Playbooks!

---

## Prereqs

* Host file in place
* Connectivity to all servers within Host file
* CentOS and/or Ubuntu client servers
* Only use Apache OR Nginx

## Playbook descriptions

### apache_update
Updates apache configuration files along with any changes to the webpages in both CentOS and Ubuntu instances

### nginx_update
Updates nginx configuration files along with any changes to the webpages in both CentOS and Ubuntu instances

---

Each playbook updates their respective configuration files and the content within the html directory.

The initial playbooks calls two roles that have the tasks within.

apache_update.yml roles:
* apache-centos
* apache-ubuntu

nginx_update.yml roles:
* nginx-centos
* nginx-ubuntu

### Roles

Each Role has five directories within that manage the required steps

* defaults

   Holds the default values for directory location on the server and other variables
* handlers
** Contains the handlers called in the tasks
* meta
** Not utilized in this setup
* tasks
** Houses all tasks to be ran within the Role
* templates
** Has the configuration templates that are used
