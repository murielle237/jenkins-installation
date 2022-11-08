# jenkins-installation
#!/bin/bash 

          #AUTHOR: murielle
          #DATE :   november 2022

 #------------ Jenkins Installation ----------------

 echo "Starting Jenkins installation . This might take few minutes......"

   yum install java-11-openjdk-devel -y
   curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
   sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
yum install jenkins -y
systemctl start jenkins
systemctl status jenkins
systemctl enable jenkins

##If you have a firewall installed, you must add Jenkins as an exception. You must change YOURPORT in the script below to the port you want to use. Port 8080 is the most common.
YOURPORT=8080
PERM="--permanent"
SERV="$PERM --service=jenkins"
firewall-cmd $PERM --new-service=jenkins
firewall-cmd $SERV --set-short="Jenkins ports"
firewall-cmd $SERV --set-description="Jenkins port exceptions"
firewall-cmd $SERV --add-port=$YOURPORT/tcp
firewall-cmd $PERM --add-service=jenkins
sudo firewall-cmd --permanent --zone=public --add-port=8080/
firewall-cmd --reload

echo "Jenkins has been installed. Thank you for your patience."
