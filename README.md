#New Docker Journey:-
Create and connect new ec2 instance
1.	Create NGS group in AWS
Inbound rule:-
Custom TCP:
-	Range 0-65000
-	Source 0.0.0.0/0
SSH:
-	Range 22
-	Source 0.0.0.0/0
 

Outbound rule:-
 
2.	Create VM with this security group
3.	Create a new key pair and download pem file
4.	Convert pem into .ppk so that vm instance can be connected from putty
-	Open puttyGen from start.
-	Click on “Load” and open the pem
-	Click on “Save private key” and save some location. It will be .ppk format. 
5.	Open Putty and and load .ppk private key to connect ec2 instance. 
 
6.	Copy public ip and connect to the system
7.	User id is:- ec2-user and then switch to “root” [sudo su]

Install Docker:
1.	yum install update
2.	yum install docker
3.	service docker start
4.	service docker status
5.	docker run hello-world
Run Jenkins as container in Docker:
1.	useradd -m -d /opt/jenkins_home jenkins -u 1200
2.	passwd Jenkins
“jenkins”
3.	Create /opt/jenkins_home
4.	chmod –R 751 jenkins_home
5.	chown –R e2-user:ec2-user jenkins_home
6.	chown –R jenkins:jenkins jenkins_home
7.	goto /home/ec2-user/ & create Dockerfile
FROM jenkins/jenkins:lts-jdk11
MAINTAINER Rahul Das
#ENV JENKINS_USER admin
#ENV JENKINS_PASS admin
ENV JENKINS_SLAVE_AGENT_PORT=49100
#ENV JAVA_OPTS -Djenkins.insall.runSetupWizard=false
EXPOSE 8080
VOLUME /var/jenkins_home
8.	docker build --no-cache -t myimagejs .
9.	docker run -d --name mycontjs -p 5000:8080 -v /opt/jenkins_home:/var/jenkins_home -u 1200:1200 myimagejs
10.	docker exec -it mycontjs bash
11.	docker container exec -it mycontjs bash
    then do the plugin and then https
myscript
USERNAME=jenkins
HOME=/opt/jenkins_home
#VOL=jenkins_home
IMAGE=myimagejs:final
CONT=mycontjs
REF_LOC=/usr/share/jenkins/ref

mkdir $HOME
chown 1000:1000 $HOME
cp casc.yaml $HOME
#####################################
# Define no of executors in Jenkins master machine
#####################################

cat <<EOF >executors.groovy
import jenkins.model.*
Jenkins.instance.setNumExecutors(4)
EOF

#####################################
# creates DockerFile
#####################################

cat <<EOF >Dockerfile
FROM jenkins/jenkins:lts-jdk11
MAINTAINER Rahul Das
ENV JENKINS_SLAVE_AGENT_PORT=49100
#ENV JENKINS_USER admin
#ENV JENKINS_PASS admin
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
ENV CASC_JENKINS_CONFIG /var/jenkins_home/casc.yaml
ENV HOME /var/jenkins_home

COPY --chown=$USERNAME:$USERNAME plugins.txt $REF_LOC
RUN xargs /usr/local/bin/install-plugins.sh < $REF_LOC/plugins.txt

COPY --chown=$USERNAME:$USERNAME executors.groovy  $REF_LOC/init.groovy.d/executors.groovy
#RUN mkdir $REF_LOC/ssl
#COPY --chown=$USERNAME:$USERNAME rahul.pfx $REF_LOC/ssl
COPY --chown=$USERNAME:$USERNAME casc.yaml /var/jenkins_home/casc.yaml
#ENV JENKINS_OPTS --httpPort=-1 --httpsPort=8081 --httpsKeyStore=$REF_LOC/ssl/rahul.pfx --httpsKeyStorePassword=Edison123
EXPOSE 8080
VOLUME $HOME
EOF

docker build --no-cache -t $IMAGE .
docker run -d --name $CONT -p 5000:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=admin -v $HOME:/var/jenkins_home $IMAGE

#### Use this if you wish to run Manually #####
#docker run -d --name mycontjs -p 5000:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=admin -v $HOME:/var/jenkins_home myimagejs:casc

### Run without bind or volume mount
#docker run -d --name mycontjs -p 5000:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=admin myimagejs:<tag> D=admin myimagejs:<tag>

++++++++++++++++++++++++++++++
plugins.txt
authorize-project:latest
pipeline-maven:3.10.0
ssh
ssh-agent
oauth-credentials
ant:latest
antisamy-markup-formatter:latest
build-timeout:latest
cloudbees-folder:latest
configuration-as-code:latest
credentials-binding:latest
email-ext:latest
git:latest
github-branch-source:latest
gradle:latest
ldap:latest
mailer:latest
matrix-auth:latest
pam-auth:latest
pipeline-github-lib:latest
pipeline-stage-view:latest
ssh-slaves:latest
timestamper:latest
workflow-aggregator:latest
ws-cleanup:latest
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
casc.yaml
jenkins:
  securityRealm:
    local:
      allowsSignup: false
      users:
       - id: ${JENKINS_ADMIN_ID}
         password: ${JENKINS_ADMIN_PASSWORD}
       - id: test
         password: test@123
  authorizationStrategy:
    projectMatrix:
      permissions:
        - "Overall/Administer:admin"
        - "Overall/Read:authenticated"
        - "Overall/Read:test"
  remotingSecurity:
    enabled: true
security:
  queueItemAuthenticator:
    authenticators:
    - global:
        strategy: triggeringUsersAuthorizationStrategy
unclassified:
  location:
    url: http://localhost:5000/
