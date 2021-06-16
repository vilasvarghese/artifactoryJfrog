#Artifactory can be installed on Linux system in one of the following ways:
	Manual Installation
	Service Installation
	RPM or Debian Installation
	As a Docker Container


#Install JDK 11
	sudo yum update
	sudo yum install java-11-openjdk-devel
	or
	sudo yum install java-11-openjdk
	java -version
	javac
	
#Setting JAVA_HOME

	sudo update-alternatives --config java
	Copy the location
/usr/lib/jvm/java-11-openjdk-11.0.10.0.9-1.el7_9.x86_64/bin/java

	Copy for retain after restart
	vi /etc/profile (or .bash_profile)
		Add 
		export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.10.0.9-1.el7_9.x86_64
		export PATH=$JAVA_HOME/bin:$PATH
		
	source /etc/profile	
	
	For current session
	export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.10.0.9-1.el7_9.x86_64
	JAVA_HOME environment variable should be set to JDK installation directory.


#Setting Java Memory Parameters
	Recommended (not mandatory) 
		modify JVM memory parameters used to run Artifactory.
		Reserve at least 512MB for Artifactory
	recommended values for JVM parameters are:
		The larger your repository or number of concurrent users, the larger you need to make the -Xms and -Xmx values accordingly. Recommended minimal values are:

		-server -Xms512m -Xmx2g -Xss256k -XX:+UseG1GC
		Exact instruction refer the precise option we are installing.
		

		
#Manual installation, 
	
	Unzip the Artifactory download file to a location on your file system. 
		This will be your $ARTIFACTORY_HOME location.
	Modify JAVA_OPTIONS in $ARTIFACTORY_HOME/bin/artifactory.default.

	Done

https://bintray.com/jfrog/product/JFrog-Artifactory-Cpp-CE/view

	
	mkdir jfrog
	cd jfrog
	
	Go to https://jfrog.com/open-source/#artifactory
	wget <<tar.gz link>>
	
	or wget https://bintray.com/jfrog/artifactory/download_file?file_path=jfrog-artifactory-cpp-ce-6.23.13.zip


	export JFROG_HOME=/root/jfrog
	export $ARTIFACTORY_HOME=/root/jfrog
										
	tar -xvf  jfrog-artifactory-ce-version-linux.tar.gz
	mv artifactory-ce-version-linux artifactory
	
	or 
	sudo yum install unzip
	unzip jfrog-artifactory-ce-version-linux.zip

	cd $JFROG_HOME/artifactory/app/bin
	 ( cd /root/jfrog/artifactory/app/bin)

	To start artifactory:
		./artifactoryctl start
		or 
		./artifactory.sh

	To check the status of artifactory
		./artifactoryctl check

	To stop the artifactory
		./artifactoryctl check|stop

	Enabling artifactory if required.
		systemctl start artifactory.service

-------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------
Add the ports 8081 and 8082 to firewall exceptions:
sudo firewall-cmd --permanent --zone=public --add-port=8081/tcp 
sudo firewall-cmd --permanent --zone=public --add-port=8082/tcp 
sudo firewall-cmd --reload
------------------------------------------------------------------------------------
	
	Running Artifactory
	-------------------
	Run Artifactory manually to see its behavior by directly executing:
		$ARTIFACTORY_HOME/bin/artifactory.sh 
		Console is locked on the Artifactory process and you can stop it cleanly with Ctrl+C.

	Run Artifactory as a daemon process, 
		using the environment variables of the shell you are currently in, execute the following script:

	$ARTIFACTORY_HOME/bin/artifactoryctl start
	To verify Artifactory is running, you can access it in your browser at:

	http://SERVER_DOMAIN:8081/artifactory

	For example, if you are testing on your local machine you would use:  http://localhost:8081/artifactory

	Startup time
	Depending on your system performance it may take Artifactory several seconds.


	Checking if Artifactory is running or stopping it
	$ARTIFACTORY_HOME/bin/artifactoryctl check | stop
	
	
#service installation, 
	Artifactory is packaged as a zip file with a bundled Tomcat
	Complete install script that can be used to install it as a service.
	When running Artifactory as a service
		installation script creates a user called Artifactory 
		Must have run and execute permissions on the installation directory.

	Recommendation 
		Extract the Artifactory download file into a directory 
		Gives run and execute permissions to all users such as 
			/opt
		
		
		modify JAVA_OPTIONS  in $ARTIFACTORY_HOME/etc/default


	#Installation Steps
	Download artificatory repo.
	$ARTIFACTORY_HOME/bin/installService.sh [USER [GROUP]]
	
	Following happens
	User creation
		Creates a default user named artifactory ($ARTIFACTORY_USER). 
		To change default user edit the $ARTIFACTORY_USER value in /etc/opt/jfrog/artifactory/default.

		Optionally run the install script to start the Artifactory service under a different user 
			by passing in the user name as the first parameter. 
			If you include the user name: optionally include a group.

		etc config
		Creates the folder /etc/opt/jfrog/artifactory, copies the configuration files there and creates a soft link in$ARTIFACTORY_HOME/etc
		etc default
		Creates the file /etc/opt/jfrog/artifactory/default containing the main environment variables needed for Artifactory to run:JAVA_HOME, ARTIFACTORY_USER, ARTIFACTORY_HOME, JAVA_OPTIONS,... 
		The /etc/opt/jfrog/artifactory/default is included at the top of artifactoryctl and can include any settings.

		To modify your JVM parameters modify JAVA_OPTIONS in /etc/opt/jfrog/artifactory/default

		systemd or init.
		If you are running on a Linux distribution that supports systemd, the install script will use it to install Artifactory - otherwise init.d will be used.

		If systemd is supported, the install script copies the service script file artifactory to /etc/systemd/system/artifactory.service

		If systemd is not supported and init.d is used, the install script copies the service script file artifactory to /etc/init.d/artifactory

		Logs folder
		Creates the folder $ARTIFACTORY_HOME/logs, makes it writable for the user ARTIFACTORY_USER and creates a soft link $ARTIFACTORY_HOME/logs/catalina.

		The $ARTIFACTORY_HOME/tomcat/logs folder is linked to $ARTIFACTORY_HOME/logs/catalina.

		Backup folder
		Creates the folder $ARTIFACTORY_HOME/backup, so you must create a link if you want this folder to point to a different place (such as /var/backup/artifactory for example). The script makes $ARTIFACTORY_HOME/backup writable for the user ARTIFACTORY_USER.
		Data folder
		Creates the folder $ARTIFACTORY_HOME/data, so you must create a link if you want this folder to point to somewhere else. The script makes it writable for the user ARTIFACTORY_USER.
		chkconfig calls
		The script calls add and list (you can see the output), and then activates the Artifactory service


#RPM or Debian installation, 
	modify JAVA_OPTIONS in /etc/opt/jfrog/artifactory/default
#Docker Container installation, 
	pass the EXTRA_JAVA_OPTIONS environment variable to the docker run command


#Steps to Install Artifactory:



------------------------------------------------------------------------
$ARTIFACTORY_HOME/bin
	.bat
		for windows
	
	
	

------------------------------------------------------------------------

http://url:8081/

http://url:8081/artifactory/webapp/