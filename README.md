# New Docker Journey:-
Create and connect new ec2 instance
1.	Create NGS group in AWS
- Inbound rule:-
   - Type: Custom TCP:
      -	Range 0-65000
      -	Source 0.0.0.0/0
   - SSH:
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
