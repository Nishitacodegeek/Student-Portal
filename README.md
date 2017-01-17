# Student-Portal
---
Discription: Student portal created for students to login to their account to access institute announcements using Joomla CMS and link to Moodle LMS for students to read their lessons
---
Steps followed:
---Migrating current mywai student portal (2.5) to a new instance (3.5) using migratemeplus on local m/c---
	Assuming we have already installed XAMPP server and enabled the ports, we clean up our current joomla instance (2.5) before migration.
	Go to extensions manager-> manage-> and uninstall all the following components:
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
	Make sure all unnecessary 3rd party extensions are removed.
	Now install migratemeplus and click on configurations. Make sure migration speed is selected as “slow” and backup is enabled. Click save and click on “migrate” button.
o	If a JSON error pops up with PHP timeout error, go to php.ini file and increase the “max_execution_time” from 30 sec to 360 sec. If error still exists, go to .htaccess file and add the following script. 
<IfModule mod_php5.c>
php_value post_max_size 200M
php_value upload_max_filesize 200M
php_value memory_limit 300M
php_value max_execution_time 259200
php_value max_input_time 259200
php_value session.gc_maxlifetime 1200
</IfModule>
o	If CURL error pops up, make sure Internet connection is on.
o	If roksprocket or rokcommom plugin warnings appear post migration in new admin panel, uninstall them and install the latest version and enable them.
	Once the migration is complete, check the back end for any errors. If there are any fatal errors regarding plugins, disable them at the backend and restart the migration process.
	Remove all old templates. Disable and delete all the modules which are not required.
	The extension manager will have the following important extensions:
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
	Now check if everything works fine and install the essential modules required.
o	Akeeba backup 5.1.4
o	Chronoforms V5
o	Docman 2.1
o	 Latest rocket theme (photon 1.0.2)
	Take a backup of this instance using Akeeba backup and upload it into server. Set up server using the following steps and unzip the backup and install the joomla instance. 
