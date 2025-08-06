# DevOps Project - Ashwini

## Deployment of Hello-World Application

This project demonstrates deploying a Hello-World application using GitHub, Maven, Docker, and Tomcat .

---

## Repository URL


---

## Prerequisites
1. **Launch an Instance:**
   - **OS:** Linux 6.1
   - **Instance Type:** t2.medium
   - **Storage:** 20 GB

2. **Connect to the Instance:**
   # switch to root-user
    sudo su

Step 1: Update Packages and Install Dependencies
1. Update default packages:
yum upgrade -y
2. Install Java 17:
sudo yum install java-17-amazon-corretto-devel -y
3. Install Maven
yum install maven -y

Step 2: Install and Configure Docker
1. Install Docker:
yum install docker -y
2. Give permissions to run Docker:
sudo chmod 666 /var/run/docker.sock
3. Enable docker
systemctl enable docker
4. Start docker
systemctl start docker
5. verify the docker status
systemctl status docker

Step 3: Install and Configure Jenkins
1. Install Jenkins : 
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
sudo yum install jenkins
sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
2. Access Jenkins:
---> Jenkins runs on port 8080 by default.
---> Open the Security Group and edit inbound rules to allow traffic on port 8080.
---> Access Jenkins at http://<public-ip>:8080.
3. Retrieve the default admin password:
cat /var/lib/jenkins/secrets/initialAdminPassword
4. Complete the Jenkins setup:
Change the default password.

Step 4: Install necessary Jenkins plugins:
Go to Manage Jenkins -> Plugins -> Available Plugins.
Install:
---> Pipeline: Stage View
---> Docker Pipeline
---> Deploy to Container
4. Configure Maven:
---> Go to Manage Jenkins -> Tools -> Add Maven.
---> Name: maven s/w.
---> Enable automatic installation.
5. Add DockerHub credentials in Jenkins:
---> Generate a personal access token from DockerHub.
---> In Jenkins:
Navigate to Manage Jenkins -> Credentials -> Global -> Add Credentials.
---> Use:
Kind: Username and Password
Username: DockerHub username.
Password: Personal access token.
ID: dockerhub-cred.

Step 5: Configure Tomcat Server
Once the container is running, modify the required Tomcat configuration files directly in the container.
1. Access the Running Container:
docker exec -it hello-world-tomcat bash
2. Modify Configuration Files:
---> Change server.xml to update the port:
Path: /usr/local/tomcat/conf/server.xml.
---> Update <Connector port="8080" to <Connector port="8081".
3. Grant admin permissions:
---> Edit context.xml
vi /path/to/tomcat/webapps/manager/META-INF/context.xml
---> Edit the allow tag -----> ".*" />
4. Configure users:
---> Edit tomcat-users.xml located in the conf folder
<role rolename="manager-gui" />
<user username="tomcat" password="tomcat" roles="manager-gui" />
<role rolename="admin-gui" />  
<user username="ashwini" password="ashwini" roles="manager-gui,admin-gui"/>
5. Restart the Tomcat Container:
docker restart hello-world-tomcat
6. Access the Application:
---> URL: <public-ip>:8081

Step 6: Create and Run the Jenkins Pipeline
1. Create a new pipeline in Jenkins:
---> Go to Dashboard -> New Item -> Pipeline.
---> Name the project Hello-World.
2. Configure the pipeline script to build and deploy the application.
3. Run the pipeline:
---> Trigger the Jenkins pipeline to build and deploy the application.
---> The Docker container will expose your application on port 8081.

Note: 
Ensure all changes to configuration files are saved before restarting services.
Troubleshooting : 
Port Conflict: Ensure Tomcat and Jenkins are running on separate ports.

A fully automated CI/CD pipeline deploying a Hello-World Java web app using Jenkins, Docker, and Tomcat.
