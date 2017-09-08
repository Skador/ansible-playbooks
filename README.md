# Playbooks!

---

## Tests

1. apache_up_cent_web.log
  * Contains the infomation for the completed centos apache configuration file and the web files update tests.
2. apache_up_ubuntu.log
  * Contains the information for the completed ubuntu apache configuration file test.
3. nginx_up_cent_web.log
  * Contains the information for the completed centos nginx configuration file and the web files update tests.
4. nginx_up_ubuntu.log
  * Contains the information for the completed ubuntu nginx configuration file test.

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

### Handler

Only handler used in this setup is the restart handler.

* apache
  * CentOS
    * Package name: httpd
    * State: restarted
  * Ubuntu
    * Package name: apache2
    * State: restarted
* nginx
  * CentOS and Ubuntu
    * Package name: nginx
    * State: restarted 

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

#### nginx
* Verify nginx repo file is present
  * CentOS
    * Uses a template to add the nginx.repo file to /etc/yum.repos.d/
  * Ubuntu
    * nginx is already available for Ubuntu
* Verify nginx is present
  * Verifies that nginx is present on the machine, can be changed to have nginx always update (When update is available)
  * CentOS
    * Package installer: yum
    * Package name: nginx
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
  * Ubuntu
    * Package installer: apt
    * Package name: nginx
	* State:
      * latest - To be updated (When update available)
	  * present - Installs if not on system. Does not update if already on the system.
* Update modified webfiles
  * CentOS and Ubuntu
    * Copies any updated or modified files from src/html directory to the correct html directory for the OS
* Update modified config
  * CentOS and Ubuntu
    * Uses a template to update the existing conf file for the specific OS
	* Both use - nginx.conf
	* Sends a notify to the "restart nginx" handler
* Ensure nginx is running
  * Last step that verifies that nginx has started and is currently running
  * CentOS and Ubuntu
    * Package name: nginx
	* State: started
