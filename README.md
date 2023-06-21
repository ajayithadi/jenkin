QA SERVER SETUP:

	Firstly go to the root user:
	 $ sudo su –
	Install Java

# sudo apt install default-jre
# sudo apt install default-jdk

#cd /

#cd /opt

#Download tomcat binary
official link from tomcat  https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.55/bin/

# wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.55/bin/apache-tomcat-9.0.55.tar.gz

	#unzip tomcat binary
# tar -zvxf apache-tomcat-9.0.55.tar.gz

	Add Execute Permission to startup.sh & shutdown.sh
#cd apache-tomcat-9.0.55
#cd bin
#chmod +x startup.sh
#chmod +x shutdown.sh

	Create link files for Tomcat Server up and Down
# ln -s /opt/apache-tomcat-9.0.55/bin/startup.sh /usr/local/bin/tomcatup
# ln -s /opt/apache-tomcat-9.0.55/bin/shutdown.sh /usr/local/bin/tomcatdown
# tomcatup

	Change Settings to Manage Tomcat
#cd ..
#comment value tag sections in below all files
# vi ./webapps/host-manager/META-INF/context.xml
# vi ./webapps/manager/META-INF/context.xml

	Update user information in tomcat-users.xml
#cd conf
# vi tomcat-users.xml


#Add below lines between <tomcat-users> tag
<role rolename="admin-gui"/>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/> 
<user username="admin" password="admin" roles="admin-gui,manager-gui,manager-script,manager-jmx,manager-status"/>
<user username="deployer" password="deployer" roles="manager-script"/>
<user username="tomcat" password="s3cret" roles="manager-gui"/>
---------------------------------------------------------------------------------------


First Start All the AWS Machines.

Connect Dev Server(start jenkins)

Stage 1 : Continuous Download START CI-CD

1) Create New item as free style project

2) Click on source code managment 

3) Select GIT

4) Enter the URL of github reposiditory
https://github.com/sunildevops77/maven.git

5) Click on apply and save

6) Run the Job

7) Check the console output.

Stage 2 : Continuous Build

Convert the java files in to artifact ( .war file)

10) Click on configure of the same job

11) Go to Build Section

12) Click on add build step

13) Click on Invoke top level maven targets

14) Enter the goal as  package

15) click on apply and save

16) Run the Job

17) Click on number & click on console output

Stage 3 :Continuous Deployment 

Now we need to deploy the war file into the QA Server.

19) For this we need to install "deploy to container" plugin.
 
Go to Dasboard

Click on manage jenkins

Click on manage plugins

Click on avaiable section

Search for plugin ( deploy to container )

Select that plugin and click on install  restart.

20) Click on post build actions of the development job

21) Click on add post build actions

22) Click on deploy war/ear to container

23) Enter the path of the war file (or)
 we can give **/*.war in war/ear files.

24) Context path: qaenv

25) Containers : select tomcat 8

Credentials : Click on add

select jenkins

enter tomcat user name and password

Click on add

Select credentials.

give the private ip of the QA server.

http://private_ip:8080
http://172.31.47.36:8080


26) Click on apply and save

27) Run the job

28) To access the home page

public_ip_Qa_server:8080/qaenv

13.127.177.32:8080/qaenv

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

https://github.com/sunildevops77/TestingNew.git


Step 1: Connect to Devserver from git bash
Step 2: Start Jenkins    ( java  -jar  jenkins.war )
Step 3: Create new item  ( Name - testing )
	Source code management tab,  Git
Repository URL - https://github.com/sunildevops77/TestingNew.git

Apply -- Save

Step 4: Run the job.

step 5: Configure the same job ( testing )
	Build -- Add build Step  -- Execute shell
 
	Command:   echo " Testing passed"



Now both are independent job.
To call testing job  after development job is completed

Go to first job  --  configure 
Post build actions -- add post build action -- build other project - 
Projects to build - testing ( name of the job)

++++++++++++++


Copying artifacts from development job to testing job

The artifacts (war) created by the development job should be passed to the testing job so that the testing job can deploy that into tomcat in the prod environment.


Install Plugins

1) Go to Jenkins dashboard

2) Go to manage jenkins

3) Click on Manage plugins

4) Search for "Copy Artifact"  plugin

5) Install the plugin

Stage 5 : Continous Delivery

1) Go to Development job 

2) Go to Configure

3) Go to Post build actions tab

4) Click on add post build action

5) Click on Archive the artifacts

6) Enter **/*.war

7) Click on apply and save

8) Go to (testing Job)

9) Click on configure

10) Go to Build section

11) Click on add build steps

12) Click on copy artifacts from another project

13) Enter Development as project name

14) For Deployment Go to Post build actions section

15) Click on add post build action

16) Click on deploy war/ear to a container

17) Enter **/*.war in war/ear files

18) Context path : prodenv

19) Click on add container 

20) Select tomcat 9

21) Select your Credentials

22) Enter private ip:8080 of the prod server

http://172.31.39.130:8080



