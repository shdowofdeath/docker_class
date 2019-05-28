## Tips and tricks 

building service api in 1 minute :) 

```

echo '{"devops": [{"name": "lola"}, {"name": "pola"}, {"name": "gola"}, {"name": "jenkins"}]}' > /tmp/devops.json
docker run -d -p 7443:80 -v /tmp/devops.json:/data/db.json elhay/api-server

# Listing my favourite cities
curl http://localhost:7443/devops [
  {"name": "lola"},
  {"name": "pola"},
  {"name": "gola"},
  {"name": "jenkins"}
]

# Listing my favourite cities containing *lo*
curl http://localhost:7443/devops?name_like=lo
[
  {"name": "lola"}
]
```
All existing containers (not only running)
```
docker ps     - show only running 
docker ps -a  - show all containers
```
Remove all containers with status=exited
when running a lot of container, can be case that exists list of containers which has status exited . Command bellow can help to remove it in one command

```
docker rm $(docker ps -q -f status=exited)
```
Stop all containers
```
docker stop $(docker ps -q)    - will run stop only for active
docker stop $(docker ps -aq) - will run stop for all
``` 
Run and attach to container ( '--rm' delete container after exit)
Pay attention! Every time with using docker run will create new container with specified image. When you are using often docker run . It will become boring after stop and remove it. Use option --rm so container will removed after it finishes. it can make your life easy.
```
docker run -it --rm <image_name> /bin/ash
```

Pass environment variables to docker
In case if you have several env variables you can use option -eor --env as example bellow.

```
docker run -it -e TEST=1234 --env TEST1=3456 --rm alpine /bin/ash
```
- write in terminal
echo $TEST   - you should have result '1234'
in case when you need to add list of variables. First example become not so comfortable. It’s better to create env.file . In docker command use option --env-file instead of -e

#Content of env.file
#--------------------
TEST=1234
TEST1=5678
example of docker command :

docker run -it --env-file ./env.list alpine /bin/ash
Remove all docker images
docker rmi $(docker images -q)
don't forget image shouldn't have reference to container 
Execute command in container
docker exec YOUR_CONTAINER echo "Hello from container!"
Bind local folder the docker folder on docker run
What if you need to share your local folder with docker container and start to using it internally from your container. you can use flag -v

```
docker run -it -v /LOCAL_PATH:/CONTAINER_PATH <container_image>
```
Example:

```
docker run -ti --rm -v /local_path:/var ubuntu
```
Build image (from folder with Dockerfile)
In case if you want to create your own image use command bellow
```
docker build -t <image_name> .
```
See logs in container
you can check logs of container. Use the following:
```
docker logs -f <container_name>
```
Believe bash is your best friend
Many developer or admins who are working close with command line create their own aliases for various commands. you can do it too, just make your working process easy. Just add these to your ~/.bashrc

#docker commands
alias dr='docker rm $(docker ps -aq)'
alias ds='docker stop $(docker ps -aq)'
alias di='docker images'
alias dri='docker rmi $(docker images -q)'
alias dsr='ds && dr'
alias dps='docker ps -a'
alias dcup='docker-compose up'
