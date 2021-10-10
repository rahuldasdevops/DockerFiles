# Docker Image: 
`docker pull dasr4/my-jenkins:v2.1` [added maven, npm as env variable]

# Run:
`docker run -d --name jenkins -p 8080:8080 --env JENKINS_ADMIN_ID=admin --env JENKINS_ADMIN_PASSWORD=XXX -v /opt/jenkins_home:/var/jenkins_home dasr4/my-jenkins:v2.1`

# [Recommendation]
Using docker compose file either in single machine or `STACK` mode:- `docker-compose up -d`.
`docker stack deploy -c docker-compose.yml jenkins`
Just need to create `user.txt` and `pwd.txt` in plain text, affter run you can remove. 
# To create new image
`bash jenkinsScript`

## Imgage URL:
[Jenkins](https://hub.docker.com/repository/docker/dasr4/my-jenkins)
