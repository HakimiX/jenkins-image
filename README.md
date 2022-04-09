# Jenkins Image 

Jenkins and Docker client

### Usage 

1. Clone the repository in the host machine 
2. Build the image: 
```shell
docker build -t jenkins-docker .
```
3. Start the container: 
```shell
docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -d jenkins-docker
```
4. Enter the container to verify that docker is accessible:
````shell
docker exec -it jenkins bash
````
5. Test 
```shell
docker ps -a 
```
See the troubleshooting section If you get the following error: 
```shell
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock:
```

### Troubleshooting 
The `Dockerfile` is adding a docker GID: 
````shell
groupadd -g 999 docker && \
````
Run the following command to figure out the GID on the host machine: 
```shell
cat /etc/group | grep docker

# output
docker:x:998:
```
change the `Dockerfile` to use 998 instead of 999 and rebuild the image in the host machine. 


**As a last resort**<br>
Run the following command to allow the whole system to write to the docker socket (which is not recommended for production setups, but just to get started).  
```shell
chmod o+rw /var/run/docker.sock 
```
Rerun the image. 
