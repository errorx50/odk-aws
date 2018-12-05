> Set up an Ubuntu Linux virtual machine on Amazon Elastic Compute Cloud
> (Amazon EC2). For instructions, see [How to Launch a Linux Virtual
> Machine](https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/)
> and select Ubuntu 16.04 LTS.
- Connect to your instance and enter the root account, start the installations process


### java 8 install

    Sudo su
    
    apt-get update
    
    apt-get upgrade
    
    add-apt-repository ppa:webupd8team/java
    
    apt-get update

> #install command

     apt-get install oracle-java8-installer

> #Check version & remove file

    java -version
    
    javac -version
    
    add-apt-repository -r ppa:webupd8team/java

> #If you run this command it will show you the Selection option and java path, so select your option 0 or 1 and also copy the path.

    update-alternatives --config java

> #copy showing path like " /usr/lib/jvm/java-8-oracle" on your clipboard and add like this 'JAVA_HOME=/usr/lib/jvm/java-8-oracle'
> 
> #Now Edit the environment 

    nano /etc/environment 
    
> #Past the JAVA_HOME location on environment file  ,

    JAVA_HOME=/usr/lib/jvm/java-8-oracle

> #Reload environment

    source /etc/environment

> #Check JAVA HOME

    echo $JAVA_HOME

### Install Apache, PHP, MySQL, phpMyAdmin

    apt-get update
    apt-get install apache2

> #check for syntax errors by typing

    apache2ctl configtest

> #Adjust the Firewall to Allow Web Traffic

    ufw app list
    ufw app info "Apache Full"
    ufw allow in "Apache Full"

> #after that restart apache2

    systemctl restart apache2

> #Check if Apache web server is running or not

    systemctl status apache2

> #Now, open up your web browser and navigate to http://localhost/ or http://IP-Address/.

### MySQL Installation

    apt-get update

> #installation command

    apt-get install mysql-server

> #Give your ROOT Password during installation process, after process done then hit this commend,

    mysql_secure_installation

> #it will asking you some questions, and type Y;
> 
> - Type Y to remove the anonymous user accounts.
> - Type Y to disable the remote root login.
> - Type Y to remove the test database.
> - Type Y to reload the privilege tables and save your changes.
> 
> #Testing MySQL

    systemctl status mysql.service

> #check version and acess database

    mysqladmin -p -u root version

> #check database

    mysql -u root -p

> #Check databases

    show databases;

> #exit database

    \q

### Install PHP

> #install command

    apt-get install php libapache2-mod-php php-mcrypt php-mysql

> #In most cases, we'll want to modify the way that Apache serves files when a directory is requested. Currently,if a user requests a
> directory from the server, Apache will first look for a file called
> index.html. We want to tell our web server to prefer PHP files, so
> we'll make Apache look for an index.php file first.
> 
> #To do this, type this command to open the dir.conf file in a text editor with root privileges:

    nano /etc/apache2/mods-enabled/dir.conf

> #It will look like this:

    /etc/apache2/mods-enabled/dir.conf
    <IfModule mod_dir.c>
        DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    </IfModule>

> #We want to move the PHP index file (index.php) to the first position after the DirectoryIndex specification, like this:

    /etc/apache2/mods-enabled/dir.conf
    <IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    </IfModule>

> #we need to restart the Apache web server in order for our changes to be recognized.

    systemctl restart apache2

    systemctl status apache2

> #Install PHP Modules

    apt-cache search php- | less
    apt-cache show php-cli
    apt-get install php-cli

> #Test PHP Processing on your Web Server

    nano /var/www/html/info.php

> #past the code

    <?php
    phpinfo();
    ?>

> #When you are finished, save and close the file. The address you want to visit will be:

    http://your_server_IP_address/info.php 

### Install phpMyAdmin

    php -v
    php -m | grep mysql
    apt-get update
    apt-get install -y phpmyadmin

> #After installing phpMyAdmin, you will be presented with the package configuration screen.
> 
> - Press the SPACE bar to place an “*” beside “apache2.”
> 
> - Press TAB to highlight “OK,” then hit ENTER.
> 
> - Select “Yes” and then hit ENTER at the dbconfig-common screen:
> 
> - You will be prompted for your database administrator’s password.
> 
> - Type it in, hit TAB to highlight “OK,” and then press ENTER.
> 
> - Next, enter a password for the phpMyAdmin application itself.
> 
> - Confirm the phpMyAdmin application password.
> 
> #Enable PHP mcrypt Module

    php -m | grep mcrypt

> #If you don’t get any results, install the PHP mcrypt module with:

    php7enmod mcrypt

> #Now when we check, you should see mcrypt enabled:

    php -m | grep mcrypt

> #'mcrypt' will be showing
> 
> #Restart Apache

    service apache2 restart

> #if you see error, then you need to update information in .confg file

    nano /etc/apache2/apache2.conf

> #Then add the following line to the end of the file

    Include /etc/phpmyadmin/apache.conf

> #Then restart apache

    /etc/init.d/apache2 restart

### TOMCAT 8 INSTALL

> #add user , i am using 'tomcat'

    groupadd tomcat

    useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

> #Find the latest version of Tomcat 8 at the Tomcat 8 Downloads page. At the time of writing, the latest version is 8.5.5, but you should
> use a later stable version if it is available. Under the Binary
> Distributions section, then under the Core list, copy the link to the
> "tar.gz".
> 
> #Next, change to the /tmp directory on your server. This is a good directory to download ephemeral items, like the Tomcat tarball, which
> we won't need after extracting the Tomcat contents:
> #I use tomcat v8.5.35.

    cd /tmp

> #Use curl to download the link that you copied from the Tomcat website, If file are not downloading from this link then browse http://www-eu.apache.org/dist/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.tar.gz link and check the file is there or not. if not  then hit this http://www-eu.apache.org/dist/tomcat/tomcat-8/ link.  you will get the Tomcat Directory

    curl -O http://www-eu.apache.org/dist/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.tar.gz


> #install Tomcat to the /opt/tomcat directory. Create the directory, then extract the archive to it with these commands:

    mkdir /opt/tomcat

    tar xzvf apache-tomcat-8.5.35.tar.gz -C /opt/tomcat --strip-components=1

> #Tomcat user that we set up needs to have access to the Tomcat installation. Change to the directory where we unpacked the Tomcat
> installation:

    cd /opt/tomcat

> #Give the tomcat group ownership over the entire installation directory

    chgrp -R tomcat /opt/tomcat
    chmod -R g+r conf
    chmod g+x conf
    chown -R tomcat webapps/ work/ temp/ logs/

> #Tomcat needs to know where Java is installed. This path is commonly referred to as "JAVA_HOME". The easiest way to look up that location
> is by running this command:

    update-java-alternatives -l

> #Output showing this path

    /usr/lib/jvm/java-8-oracle/

> #Your JAVA_HOME may be different.
> 
> #The correct JAVA_HOME variable can be constructed by taking the output from the last column and appending /jre to
> the end. Given the example above, the correct JAVA_HOME for this
> server would be:

    /usr/lib/jvm/java-8-oracle/jre

> #Open a file called tomcat.service

    nano /etc/systemd/system/tomcat.service

> #Paste the following contents into your service file. Modify the value of JAVA_HOME if necessary to match the value you found on your system. file looks like this

    [Unit]
    Description=Apache Tomcat Web Application Container
    After=network.target
    
    [Service]
    Type=forking
    
    Environment=JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre 
    Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
    Environment=CATALINA_HOME=/opt/tomcat
    Environment=CATALINA_BASE=/opt/tomcat
    Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
    Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
    
    ExecStart=/opt/tomcat/bin/startup.sh
    ExecStop=/opt/tomcat/bin/shutdown.sh
    
    User=tomcat
    Group=tomcat
    UMask=0007
    RestartSec=10
    Restart=always
    
    [Install]
    WantedBy=multi-user.target

> #Save the file and reload the systemd daemon so that it knows about our service file & Start the Tomcat service:

    systemctl daemon-reload
    systemctl start tomcat
    systemctl status tomcat

> #Allow traffic to that port by typing:

    ufw allow 8080

> #enable the service file so that Tomcat automatically starts at boot:

    systemctl enable tomcat
    systemctl restart tomcat

> #Configure Tomcat Web Management Interface

    nano /opt/tomcat/conf/tomcat-users.xml

> #You can do so by defining a user, similar to the example below, between the tomcat-users tags. Be sure to change the username and
> password to something secure:


    <role rolename="admin-gui"/>
    <role rolename="manager-gui"/>
    <user username="tomcat" password="tomcat" roles="admin-gui,manager-gui"/>

> #By default, newer versions of Tomcat restrict access to the Manager and Host Manager apps to connections coming from the server itself.
> Since we are installing on a remote machine, you will probably want to
> remove or alter this restriction. To change the IP address
> restrictions on these, open the appropriate context.xml files.
> 
> #Inside, comment out the IP address restriction to allow connections from anywhere. Alternatively, if you would like to allow access only
> to connections coming from your own IP address,you can add your public
> IP address to the list:
> 
> #For the Manager app, type:

    nano /opt/tomcat/webapps/manager/META-INF/context.xml

> #update info and For the Host Manager app, type:

    nano /opt/tomcat/webapps/host-manager/META-INF/context.xml

> #both are look this, and update value

    <Context antiResourceLocking="false" privileged="true" >
      <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
    </Context>

> #To put our changes into effect, restart the Tomcat service:

    systemctl restart tomcat

> #Access the Web Interface

    http://server_domain_or_IP:8080

### ODK Aggregate Install

> #Now install ODK Aggregate your local computer (not on your AWS instance). For me i am using ODK-Aggregate-v1.7.0-Windows version. you
> can download the latest version and install in your local computer.
> You can find aggregate releases on

`https://github.com/opendatakit/aggregate/releases`

> #During installation process it will also ask the url, port number ,
> database name, user etc.
> - Installation will create two file named,

    - create_db_and_user.sql
    - ODKAggregate.war

> #You have to also download "mysql-connector-java-8.0.11.jar" or upgrade version file from java website.

    https://dev.mysql.com/downloads/file/?id=480292

> #Move this three file to the instance home directory from local windows
> directory. i am using  [WinSCP](https://winscp.net/eng/download.php)
> software for transfer files.

    -   create_db_and_user.sql,
    -   ODKAggregate.war,
    -   mysql-connector-java-8.0.11.jar

> #for check files type 'ls' on terminal

    ls

> #Then cp to the following folder

    cp mysql-connector-java-8.0.11.jar /opt/tomcat/lib/
    cp ODKAggregate.war /opt/tomcat/webapps/

> #Go to mysql window and run create_db_and_user.sql file, it will create automatically odk databases.

    mysql -u root -p 

> #Hit the commend for database import

    source create_db_and_user.sql

> #To put our changes into effect, restart the Tomcat service:

    systemctl restart tomcat

> #Now go to the web link and hit enter

    http://server_domain_or_IP:8080/ODKAggregate

## Done, Thank you
