# Docker Image:
docker pull dasr4/my-jenkins:v1

# Run:
docker run -d --name jenkins -p 8080:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=admin -v /opt/jenkins_home:/var/jenkins_home dasr4/my-jenkins:v1
