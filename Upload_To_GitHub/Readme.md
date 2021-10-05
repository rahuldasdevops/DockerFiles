# Upload image in GitHub repo as package

 1. Login into Github package-> docker login docker.pkg.github.com -u rahuldasdevops -p XXXXXXXXXXXXXXXXXXXX
 2. Pulling image from hub-> docker pull image node
 3. docker tag -> docker tag <imagename:tag> docker.pkg.github.com/rahuldasdevops/docker-images/<imagename:tag>
 4. docker push-> docker push docker.pkg.github.com/rahuldasdevops/docker-images/<imagename:tag>
 
 Note:- login token should have read:packages,write:packages access
 
 https://github.com/rahuldasdevops/docker-images/packages
```
docker login docker.pkg.github.com -u rahuldasdevops -p <PAT of github>
docker pull image node
docker tag node:latest docker.pkg.github.com/rahuldasdevops/docker-images/node:latest
docker push docker.pkg.github.com/rahuldasdevops/docker-images/node:latest
 ```
