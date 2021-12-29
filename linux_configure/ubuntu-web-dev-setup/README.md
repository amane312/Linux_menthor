Changelog:
  - Apr 27, 2012: Updated segments for Ubuntu 12.04


Install Ubuntu
--------------

    $ sudo apt-get update
    $ sudo apt-get upgrade





LAMP
----

1. Install LAMP

	`$ sudo apt-get install lamp-server^`

2. Enable usermod to use user's home diretory in Apache

    `$ sudo a2enmod userdir`<br>
	`$ sudo service apache2 restart`

3. Edit /etc/apache2/mods-enabled/php5.conf and comment `php_admin_value engine Off` of the line in the following code - http://forums.digitalpoint.com/showthread.php?t=1736370#post13839938

    `<IfModule mod_userdir.c>`<br>
    `   <Directory /home/*/public_html>`<br>
    `#      php_admin_value engine Off`<br>
    `    </Directory>`<br>
    `</IfModule>`<br>

4. Run Apache as yourself, add the following to end of /etc/apache2/httpd.conf - http://ubuntuforums.org/showthread.php?t=809934

	`ServerName localhost`
    `User username`<br>
	`Group username`<br>

5. Set Userdir as default localhost - https://help.ubuntu.com/community/ApacheMySQLPHP#Virtual%20Hosts\

    `$ sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/username`<br>
    `$ sudo nano /etc/apache2/sites-available/username`<br>
    - Change the `DocumentRoot` to point to the new location. For example, `/home/username/public_html/`<br>
    - Change the `Directory` directive, replace `<Directory /var/www/>` with `<Directory /home/username/public_html/>`<br>

    - Disable default site and enable yours
    `$ sudo a2dissite default && sudo a2ensite username`

6. Specify DirectoryIndex of files - https://help.ubuntu.com/10.04/serverguide/C/httpd.html

    `$ sudo nano /etc/apache2/sites-available/username`

    - add the following to below allow from all to define execution order of files

    `DirectoryIndex index.html index.htm default.htm index.php default.php kickstart.php`


7. Enable mod_rewrite http://www.ghacks.net/2009/12/05/enable-mod_rewrite-in-a-ubuntu-server/

    `$ sudo a2enmod rewrite`

    - edit /etc/apache2/sites-enabled/username

    First look in the `<Directory />` section and change the line:

    `AllowOverride None`
    to
    `AllowOverride All`

    Do the same for the `<Directory /var/www/>` section.

    Once you have the file edited, restart Apache with the command:

    `$ sudo service apache2 restart`

8. Install phpMyAdmin - https://help.ubuntu.com/community/phpMyAdmin

    `$ sudo apt-get install phpmyadmin`

    - Add the following line to /etc/apache2/apache2.conf - https://help.ubuntu.com/community/phpMyAdmin

	`Include /etc/phpmyadmin/apache.conf`

9. Set up mail server to send email to the real world - http://www.glorat.net/2008/11/ubuntu-804-hardy-gmail-smarthost-setup-with-exim4.html

    `$ sudo apt-get install exim4`<br>
    `$ sudo dpkg-reconfigure exim4-config`

    - Choose mail sent by smarthost; no local mail
    - System mail name: Your chosen host name
    - IP-addresses to listen on: 127.0.0.1 (You don't want to allow external connections!)
    - Other destinations for which mail is accepted: Leave blank
    - Visible domain name for local users: Your chosen host name
    - IP address or host name of the outgoing smarthost:  smtp.gmail.com::587
    - Keep number of DNS-queries minimal (Dial-on-Demand)? No
    - Split configuration into small files? Yes
    - Root and postmaster mail recipient: Leave blank

    `$ sudo nano /etc/exim4/passwd.client`

    And add the following lines, substituting yourAccountName and y0uRpaSsw0RD as appropriate

    `gmail-smtp.l.google.com:yourAccountName@gmail.com:y0uRpaSsw0RD`<br>
    `*.google.com:yourAccountName@gmail.com:y0uRpaSsw0RD`<br>
    `smtp.gmail.com:yourAccountName@gmail.com:y0uRpaSsw0RD`

    `$ update-exim4.conf`<br>
    `$ sudo /etc/init.d/exim4 restart` just for good measure




Tools
-----

1. Install "open in terminal" menu context http://ubuntu-tutorials.com/2007/05/13/nautilus-open-terminal-terminal-quick-launch/

    `$ sudo apt-get install nautilus-open-terminal`

2. Add Gimp "save for web" http://blog.sudobits.com/2010/09/06/gimp-save-for-web-plugin-image-optimization-on-ubuntu/

     Open Synaptic package manager and search for ‘gimp plugin registry’

3. Install VirtualBox - https://help.ubuntu.com/community/VirtualBox
    - Download  from http://www.virtualbox.org/wiki/Linux_Downloads for USB support
	- if VT-X issue
	`$ VBoxManage modifyvm virtualmachinename --hwvirtex off`
	- Install guest additions

	1. Launch VB
	2. Devices -> Install Guest Additions

    - Share host folder
    `VBoxManage sharedfolder add "XP" -name "share" -hostpath /home/your/shared/directory/VirtualBoxShare/`

4. Install rpl package (cli find/replace in multiple files)- http://www.laffeycomputer.com/rpl.html

    `$ sudo apt-get install rpl`

5. Install PEAR
    - Synaptic Package Manager `php-pear`

6. Install Phing - http://www.phing.info/trac/wiki/Users/Download

7. Install s3cmd package

    Import S3tools signing key:
        `$ wget -O- -q http://s3tools.org/repo/deb-all/stable/s3tools.key | sudo apt-key add -`

    Add the repo to sources.list:
        `$ sudo wget -O/etc/apt/sources.list.d/s3tools.list http://s3tools.org/repo/deb-all/stable/s3tools.list`

    Refresh package cache and install the newest s3cmd:
        `$ sudo apt-get update && sudo apt-get install s3cmd`


GIT
---

Set up Git

   `$ sudo apt-get install git-core`


Gedit
-----

1. Advanced find / replace plugin for gedit (Gnome Text Editor) - http://code.google.com/p/advanced-find/

     - download and Extract folder to directory ~/.gnome2/gedit/plugins

2. Transform Gedit Into A Web Developer IDE - http://maketecheasier.com/transform-gedit-into-a-web-developer-ide/2010/12/29

    `$ sudo apt-get install gedit-plugins`

3.   Install gedit-Gmate - Gmate is an additional set of plugin for gedit to make it more similar to TextMate. It contains code snippets, plugins, and an automatic registration of rails-related files.

    `$ sudo apt-add-repository ppa:ubuntu-on-rails/ppa`<br>
    `$ sudo apt-get update`<br>
    `$ sudo apt-get install gedit-gmate exuberant-ctags`

4. Gedit plugin to format, minify, and validate javascript and CSS

    PREREQUISITES - NodeJS: http://nodejs.org/

    `$ sudo apt-get install nodejs`

    INSTALL

    `$ git clone -b Gedit2 https://github.com/trentrichardson/Gedit-Clientside-Plugin.git`

    - Copy the clientside directory and clientside.gedit-plugin file into your gedit plugins directory (/usr/lib/gedit/plugins/).
    - Start or restart gedit
    - Open the Preferences, and navigate to Plugins, check to enable Clientside plugin

* More Gedit Plugins - http://live.gnome.org/Gedit/Plugins

5. Gedit themes - https://github.com/mig/gedit-themes

    `$ git clone https://github.com/mig/gedit-themes.git ~/.gnome2/gedit/styles`<br>
    `$ cd ~/.gnome2/gedit/gedit/styles`<br>



Networking
--------

1. Install Samba - http://ubuntuforums.org/showpost.php?p=7576893&postcount=6

	System --> Administration --> Synaptic Package Manager and install or check these: smbclient, libsmbclient, samba-common, nautilus-share and samba

2. Change workgroup name - http://www.liberiangeek.net/2011/03/change-workgroup-ubuntu-11-04-natty-narwhal/

	`$ sudo nano /etc/samba/smb.conf`

3. Set smbpasswd if sharing folder - https://help.ubuntu.com/8.04/internet/C/networking-shares.html

	`$ sudo smbpasswd -a username`

4. Install Openssh-server - https://help.ubuntu.com/community/SSH?action=show&redirect=SSHHowto




 DNS
---------

1. Edit resolv.conf to set DNS to use Google

	`$ sudo nano /etc/resolv.conf`

	`nameserver 8.8.8.8`<br>
	`nameserver 8.8.4.4`

2. Prevent file from being overwritten

	`$ sudo chattr +i /etc/resolv.conf`


Install Oracle Java - from http://forums.linuxmint.com/viewtopic.php?f=42&t=93052
---------
	`$ echo "deb http://www.duinsoft.nl/pkg debs all" | sudo tee /etc/apt/sources.list.d/oracle-java.list`
	`$ sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 5CB26B26`
	`$ sudo apt-get update`
	`$ sudo apt-get install update-sun-jre`
	`$ sudo update-sun-jre -v -i install`

	`$ java -version` should give us `java version "1.6.0_31"`



Awesome Apps
------------

- Keepass (http://sourceforge.net/projects/keepass/forums/forum/329220/topic/4503818)
    `$ sudo apt-get install keepass2`
- Filezila
    `$ sudo aptitude install filezilla`
- Gimp
    `$ sudo apt-get install gimp`
- Inkscape
    `$ sudo apt-get install inkscape`
- Trimage image compressor
    `$ sudo apt-get install trimage`
- Dragondisk Amazon S3 Client - http://www.dragondisk.com/
- Linkchecker
    `$ sudo apt-get install linkchecker-gui`
- Giggle - graphical frontend for git tracker
    `$ apt-get install giggle`
- Meld diff viewer
    `$ apt-get install meld`
- PhpStorm from http://www.jetbrains.com/
- Kxb CD / DVD creator
- XVidCap
- AcidRip
- LibreOffice
  - Add LibreOffice repo `sudo apt-add-repository ppa:libreoffice/ppa`

