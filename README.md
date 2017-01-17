# Student-Portal
---
Discription: Student portal created for students to login to their account to access institute announcements using Joomla CMS and link to Moodle LMS for students to read their lessons
---
Migrating current mywai student portal (2.5) to a new instance (3.5) using migratemeplus on local m/c
---
* Assuming we have already installed XAMPP server and enabled the ports, we clean up our current joomla instance (2.5) before migration.
* Go to extensions manager-> manage-> and uninstall all the following components:
1.	All Joomdle plugins and components.
2.	All Jomsocial extensions
3.	Jomsocial menus
4.	LDAP extensions from shmanic
5.	“course groups” component and plugin
6.	Teacher courses and My courses plugin
7.	Akeeba backup extensions
8.	“NoNumber” extensions
9.	“Sh404sef” extensions
10.	Docman extensions 
11.	 All JCal extensions
*	Make sure all unnecessary 3rd party extensions are removed.
*	Now install migratemeplus and click on configurations. Make sure migration speed is selected as “slow” and backup is enabled. Click save and click on “migrate” button.
   *	If CURL error pops up, make sure Internet connection is on.
   *	If roksprocket or rokcommom plugin warnings appear post migration in new admin panel, uninstall them and install the latest version and enable them.
   * If a JSON error pops up with PHP timeout error, go to php.ini file and increase the “max_execution_time” from 30 sec to 360 sec. If error still exists, go to .htaccess file and add the following script. 
   <IfModule mod_php5.c>
   php_value post_max_size 200M
   php_value upload_max_filesize 200M
   php_value memory_limit 300M
   php_value max_execution_time 259200
   php_value max_input_time 259200
   php_value session.gc_maxlifetime 1200
   </IfModule>
*	Once the migration is complete, check the back end for any errors. If there are any fatal errors regarding plugins, disable them at the backend and restart the migration process.
*	Remove all old templates. Disable and delete all the modules which are not required.
*	The extension manager will have the following important extensions:
o	Articles
o	Banners
o	Buttons
o	Editors
o	K2 items
o	RokCandy 2.0.2
o	Rokcommom 3.2.4
o	RokMiniEvents 2.0.3
o	RokModule 1.3
o	RokNavMenu 2.0.8
o	RokSprocket 2.1.14
*	Now check if everything works fine and install the essential modules required.
o	Akeeba backup 5.1.4
o	Chronoforms V5
o	Docman 2.1
o	 Latest rocket theme (photon 1.0.2).Take a backup of this instance using Akeeba backup and upload it into server. Set up server using the following steps and unzip the backup and install the joomla instance. 

---
Steps followed while hosting mywai student portal on VMs.
---
* Create virtual host and make directories for mywai portal and create databases.
*	Make directories and change the ownership of the root folder.
         mkdir -p /var/site
          mkdir /var/site/backup               		
              mkdir /var/site/www 	       		
                  chown -R apache:apache /var/site/www     	 
*	Create a new config file:
      sudo vim /etc/httpd/sites-available/site.conf		
*	 Add the command in conf file and save and close
*	Enable your virtual host file with a sym link to the sites-enabled directory:
      sudo ln -s /etc/httpd/sites-available/site.conf  /etc/httpd/sites-enabled/site.conf 	
*	Restart Apache:
      sudo service httpd restart
*	Create database:   db_2016                         	
*	Create user: admin@localhost;		
*	Set password for:  admin@localhost = PASSWORD("2016dbe");	
*	GRANT ALL ON:  db_2016.* TO admin@localhost IDENTIFIED BY '2016dbeI';  
*	Flush the privileges: FLUSH PRIVILEGES;
*	Exit putty: Exit

---
Host file setup
---
*	Once centOS is configured and website is added to server and it’s up & running, modify the host files so that my board site is pointing to the URL on server. 
*	Go to All programs-> accessories-> notepad-> right click-> run as admin-> file-> open-> C:\Windows\System32\Drivers\etc\Hosts -> add IP address and -> save
*	Now we should be able to access the url as : http://site.com.au
*	Login at the admin panel as super user and upgrade any extensions suggested in control panel. Next check for updates on joomla site. If available, upgrade them too.

      o	If your PHP is obsolete, then it can create problems while upgrading joomla to the latest version as it tries to take Akeeba backup which doesn’t work with PHP less than 5.6 versions. In such case, uninstall all the Akeeba components by going to extensions-> manage-> manage-> uninstall all the Akeeba components. Next click on “upgrade joomla” in control panel, click on upgrade.
      
      o	If there is an error with “unable to write to tmp directory” then create a /tmp folder and ensure that apache and your account both have write permissions on this folder.
*	Now install the fresh Akeeba backup on the joomla admin panel.

---
Configure LDAP
---

*	Download the shmanic platform (pkg_shplatform.zip) for joomla 3.x from http://shmanic.com/tools/jmapmyldap/download.htm
*	Enable the plugin in plugin manager (extensions-> plugin-> enable) and then access the ‘shmanic config’ in components tab.
*	Set ‘Yes’ in enable platforms tab and select both LDAP and SSO.
*	Next download Shmanic LDAP & SSO Core files (pkg_ldap_sso_core.zip), enable the plugin in plugin manager (extensions-> plugin-> enable) and then access the ‘shmanic LDAP’ in components tab.
*	Now go to ‘LDAP Host Configurations’ and create new. 
*	Enter the following details for all the parameters:

 	Name: AD
 	Status : Enabled
 	Ordering : AD
 	LDAP V3 : Yes
 	Negotiate TLS : No
 	Use Referrals : No
 	Host : ( Given by host ) 
 	Port : ( Given by host ) 
 	Proxy server : ( Given by host ) 
 	Proxy Password :  ( Given by host ) 
 	Base DN: ( Given by host ) 
 	User DN/Filter : ( Given by host ) 
 	Map User ID : ( Given by host ) 
 	Map full name : ( Given by host ) 
 	Map Email : mail
 	Test Debug : Yes
 	Give a student username and password and press debug. It should not throw any errors.
 	
*	Now we got to map the groups. Go to Extensions-> plugins-> LDAP group mapping. Enter the following details for all the parameters:

 	Sync on login : Yes
 	Abort login : Yes
 	Sync Groups : Push and Pull
 	Allow Additions : Yes
 	Allow Removals : Yes & default managed
 	Unmanaged Groups : ( Given by host ) 
 	Registered Groups : Registered
 	Mapping list :( Given by host )  
 	Validate DNs : Yes
 	Lookup type : Forward
 	Memberof Attribute : memberOf
 	Member Attribute : member
 	Member DN attribute : dn 
*	Save and close. Now you should be able to login as higher Ed student, culinary student etc. and accordingly your permissions will be set.
*	To check error files of LDAP in VM, go to administrator/logs/errors.php
5)	Joomdle Configuration ( I did this on my local m/c before moving the instance on to server, so repeat the same process on server instance as well)
*	Download the fresh Moodle (3.1) on your local m/c and install the same.
*	Follow the steps http://www.joomdle.com/wiki/Preparing_Moodle_20
*	Follow the steps from http://www.joomdle.com/wiki/Installing_Joomdle_in_Moodle_2 and http://www.joomdle.com/wiki/Installing_Joomdle_in_Joomla simultaneously as we got to copy paste token generated from joomla in Moodle and vice versa.
*	Go to extensions->modules->’Joomdle courses’. Create a new course called ‘List of available Moodle subjects (cookery)’, link to Moodle courses, select categories that we have created in Moodle and assign it to home page. Now create a K2 item and load the module here.
*	Create a menu item called ‘def icon’, select a K2 item type and select ‘List of Available Subjects (def)’ and give a template to it. Save and close.
*	Now add the menu URL to the ‘def icon’ module.
* Create a new module ‘Joomdle My Courses’. Now configure it. Group by Category: Yes, Link to ‘Moodle Courses’ and select a position, assign to home and publish. Save and Close.
*	Install Roksprocket  version and enable all plugins.
*	Configure roksprocket module 
*	Go to extensions-> modules-> new modules-> roksprocket module -> save as “FP Roksprocket Lists (news)”. Select list and K2 provider and click save. Assign a position and select the page where it should display. 
* Add the filter as category and select all categories. Now it displays list of all news items. 
* Create similar roksprocket modules called “FP Roksprocket Lists Most Popular - under tabs (news)” & “FP Roksprocket Lists Most Recent- under tabs (news)” and then create a roksprocket module with simple provider and tab layout and assign “FP Roksprocket Lists Most Popular - under tabs (news)” & “FP Roksprocket Lists Most Recent- under tabs (news)” to “roktabs - roklist (news)”. Assign it to a position in photon news template.
*	Add the custom CSS in backend : /var/site/www/templates/rt_topaz/custom/scss/
* Change to this directory and create a file called custom.scss and add CSS codes. Recompile CSS in admin panel for ‘base outline’ template and see the changes reflected.

Screenshot of a working student portal "Home" Page during iteration 1

![image](https://cloud.githubusercontent.com/assets/15920562/22004676/a0fdec8a-dcb1-11e6-9a07-845b5b87dff8.png)

Screenshot of a working student portal "News" Page during iteration 1

![image](https://cloud.githubusercontent.com/assets/15920562/22004907/6dec2490-dcb3-11e6-87a5-77b8600f73cd.png)
