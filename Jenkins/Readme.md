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


# RUN jenkins in multi container mode

1. Spin up 1 master, 2 agent machines in aws
2. Initiate swarm `docker swarm init -advertise-addr <privateip>` in master 
3. Join agent in that same cluster
4. Drain master's availability so that no service/container can run on master node. Run the command in master 
    `docker node update --availability drain <leader/manager node name>`
5. Initiate jenkins service as stack. `docker stack deploy -c docker-compose.yml firstStack`. [NOTE: Scale upto 1 as of now]
6. Initate LB and add above 3 machine's private ips and ports. Follow [LoadBalance](https://github.com/rahuldasdevops/DockerFiles/tree/main/LoadBalancer#readme)

    Only challenge as of now is the docker volume. If you scale up to 1, and you have 2 nodes. Suppose 1 node(primary) goes down, a new service will spin up in other node. Then you can only see the contents of node2, you cant see latest jobs, history as volume is not persistent. 
