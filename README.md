# New Docker Journey:- ![alt text](https://github.com/rahuldasdevops/DockerFiles/blob/main/docker.jpg?raw=true)
Create and connect new ec2 instance
1.	Create NGS group in AWS
- Inbound rule:-
```sh
   - Type: Custom TCP:
      -	Range 0-65000
      -	Source 0.0.0.0/0
   - SSH:
      -	Range 22
      -	Source 0.0.0.0/0
 ```

- Outbound rule:-
```sh
```
 
2.	Create VM with this security group
3.	Create a new key pair and download pem file
4.	Convert pem into .ppk so that vm instance can be connected from putty
   -	Open puttyGen from start.
   -	Click on `“Load”` and open the pem
   -	Click on `“Save private key”` and save some location. It will be .ppk format. 
5.	Open Putty and and load .ppk private key to connect ec2 instance. 
6.	Copy public ip and connect to the system
7.	User id is:- `ec2-user` and then switch to “root” [sudo su]

# Install Docker:
1.	`yum install update`
2.	`yum install docker`
3.	`service docker start`
4.	`service docker status`
5.	`docker run hello-world`
# Install Docker-compose:
```js
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose –version
```
# Docker Compose:
Using docker compose you can build or pull the images and achieve micro service in single node. It does not support swarm. 
Command:
   -	`docker-compose up –d`
   -	`docker-compose up –d –build` This command to build new application image in runtime without stopping anything.
# Docker Swarm:
There are some prerequisites to start swarm:
```
TCP port 2377 for cluster management communications
TCP and UDP port 7946 for communication among nodes
UDP port 4789 for overlay network traffic
```
Using docker swarm, you can handle multi container in multiple docker hosts/engines. 
- `docker swarm init` - for initialize swarm, and the node will become "Leader". You can also run `docker swarm init --advertise-addr <privateip>`.
- `docker swarm init --advertise-addr <privateip> --data-path-port <some customize port>` -- [StackOver Flow1](https://stackoverflow.com/questions/60438128/swarm-mode-routing-mesh-not-working-instead-is-working-like-host-mode-by-defaul) [StackOver Flow2](https://stackoverflow.com/questions/57151496/is-it-possible-to-customize-swarm-port-if-so-how-to-do-this)
-  Either you can add node as manager or worker with Leader in swarn network. 
- `docker swarm join-token <worker/manager>` to Get the token.
- `doker swarm leave --force` to leave swarm.
- `promote` to make worker to manager or `demote` to manager to worker.
- `docker run -it -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer` to create dashboard view. Not in swarm mode. 
- `docker node update --availability drain <leader/manager node name>` to disable manager or leader node to run any container. Only worker node will run the application service as container. 

# Docker stack:
Using stack, if you can achieve mircoservice in multi containers. 
   - 	`docker stack deploy -c docker-compose.yml firstStack`
   - 	`docker stack services firstStack`
   -	`docker service ls`
   
# Docker networking:
After you install 3 type of drivers come-
   -	Bridge (default) -> Isolated from host, private network. Need to do port forwarding to map docker private port with host port. 
   -	Host -> Uses host network. 
   -  None -> Uses no network. This is using when you want to discard container from any network. 
- Overlay network: - It uses in multi hosts, multi containers communication. 

   After you initialize swarm addition 2 network comes:-
   -	Ingress: - This is overlay type network uses for load balancing in multiple hosts. It uses routing mesh for LB. 
   -	Gwbridge:- This is bridge type network. 

When you initialize a swarm or join a Docker host to an existing swarm, two new networks are created on that Docker host:
   -	an overlay network called ingress, which handles the control and data traffic related to swarm services. When you create a swarm service and do not connect it to a user-defined overlay network, it connects to the ingress network by default.
   -	a bridge network called docker_gwbridge, which connects the individual Docker daemon to the other daemons participating in the swarm.

To check network:
   -	`docker network ls`
   -	`docker network inspect <n/w name>`

