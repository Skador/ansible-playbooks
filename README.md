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
  * Holds the default values for directory location on the server and other variables
* handlers
  * Contains the handlers called in the tasks
* meta
  * Not utilized in this setup
* tasks
  * Houses all tasks to be ran within the Role
* templates
  * Has the configuration templates that are used

---

### Tasks

The Tasks utilized don't differ too much from each OS. Either the package name is different, more packages are needed, or files need to be placed in a different location.

#### apache

* Verify apache is present
  * Verifies that apache is present on the machine, can be changed to have apache always update (When update is available)
  * CentOS
    * Package installer: yum
    * Package name: httpd
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
  * Ubuntu
    * Package installer: apt
    * Package name: apache2
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
    * Package installer: apt
	* Package name: apache2-doc
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
    * Package installer: apt
	* Package name: apache2-utils
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
* Update modified webfiles
  * CentOS and Ubuntu
    * Copies any updated or modified files from src/html directory to the correct html directory for the OS
* Update modified config
  * CentOS and Ubuntu
    * Uses a template to update the existing conf file for the specific OS
	* CentOS - httpd.conf
	* Ubuntu - apache2.
	* Sends a notify to the "restart apache" handler
* Ensure apache is running
  * Last step that verifies that apache has started and is currently running
  * CentOS
    * Package name: httpd
	* State: started
  * Ubuntu
    * Package name: apache2
    * State: started

---
