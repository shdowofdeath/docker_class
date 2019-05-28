# Docker Images
We've looked at images before, but in this section we'll dive deeper into what Docker images are and build our own image! Lastly, we'll also use that image to run our application locally and finally deploy on AWS to share it with our friends! Excited? Great! Let's get started.

Docker images are the basis of containers. In the previous example, we pulled the Busybox image from the registry and asked the Docker client to run a container based on that image. To see the list of images that are available locally, use the docker images command.

$ docker images
REPOSITORY                      TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
python                          3-onbuild           cf4002b2c383        5 days ago          688.8 MB
martin/docker-cleanup-volumes   latest              b42990daaca2        7 weeks ago         22.14 MB
ubuntu                          latest              e9ae3c220b23        7 weeks ago         187.9 MB
busybox                         latest              c51f86c28340        9 weeks ago         1.109 MB
hello-world                     latest              0a6ba66e537a        11 weeks ago        960 B
The above gives a list of images that I've pulled from the registry, along with ones that I've created myself (we'll shortly see how). The TAG refers to a particular snapshot of the image and the IMAGE ID is the corresponding unique identifier for that image.

For simplicity, you can think of an image akin to a git repository - images can be committed with changes and have multiple versions. If you don't provide a specific version number, the client defaults to latest. For example, you can pull a specific version of ubuntu image

``` 
docker pull ubuntu:latest
``` 
To get a new Docker image you can either get it from a registry (such as the Docker Hub) or create your own. There are tens of thousands of images available on Docker Hub. You can also search for images directly from the command line using docker search.

An important distinction to be aware of when it comes to images is the difference between base and child images.

Base images are images that have no parent image, usually images with an OS like ubuntu, busybox or debian.

Child images are images that build on base images and add additional functionality.

Then there are official and user images, which can be both base and child images.

Official images are images that are officially maintained and supported by the folks at Docker. These are typically one word long. In the list of images above, the python, ubuntu, busybox and hello-world images are official images.

User images are images created and shared by users like you and me. They build on base images and add additional functionality. Typically, these are formatted as user/image-name.

# Docker build my first docker image and push it 
in this folder you can easly find flask python application 

than let's build it :)
```
docker build -f thisIsDockerFile -t elhay/myfirstimgaeimsocoool:test1 .
Sending build context to Docker daemon  8.704kB
Step 1/9 : FROM ubuntu:latest
 ---> cd6d8154f1e1
Step 2/9 : MAINTAINER Elhay Efrat  "elhay.efrat@gmail.com"
 ---> Using cache
 ---> ab464a4b6d3a
Step 3/9 : RUN apt-get update -y
 ---> Using cache
 ---> 404018ca13da
Step 4/9 : RUN apt-get install -y python-pip python-dev build-essential
 ---> Using cache
 ---> 401b23774bfd
Step 5/9 : COPY . /app
 ---> c09ebe31bd5d
Step 6/9 : WORKDIR /app
 ---> Running in 7eea688ad5a0
Removing intermediate container 7eea688ad5a0
 ---> c47405d7b149
Step 7/9 : RUN pip install -r requirements.txt
 ---> Running in de796d674225
Collecting Flask==1.0.2 (from -r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/7f/e7/08578774ed4536d3242b14dacb4696386634607af824ea997202cd0edb4b/Flask-1.0.2-py2.py3-none-any.whl (91kB)
Collecting Werkzeug>=0.14 (from Flask==1.0.2->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/9f/57/92a497e38161ce40606c27a86759c6b92dd34fcdb33f64171ec559257c02/Werkzeug-0.15.4-py2.py3-none-any.whl (327kB)
Collecting click>=5.1 (from Flask==1.0.2->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl (81kB)
Collecting Jinja2>=2.10 (from Flask==1.0.2->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/1d/e7/fd8b501e7a6dfe492a433deb7b9d833d39ca74916fa8bc63dd1a4947a671/Jinja2-2.10.1-py2.py3-none-any.whl (124kB)
Collecting itsdangerous>=0.24 (from Flask==1.0.2->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
Collecting MarkupSafe>=0.23 (from Jinja2>=2.10->Flask==1.0.2->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/fb/40/f3adb7cf24a8012813c5edb20329eb22d5d8e2a0ecf73d21d6b85865da11/MarkupSafe-1.1.1-cp27-cp27mu-manylinux1_x86_64.whl
Installing collected packages: Werkzeug, click, MarkupSafe, Jinja2, itsdangerous, Flask
Successfully installed Flask-1.0.2 Jinja2-2.10.1 MarkupSafe-1.1.1 Werkzeug-0.15.4 click-7.0 itsdangerous-1.1.0
Removing intermediate container de796d674225
 ---> 5188eb8d6247
Step 8/9 : ENTRYPOINT ["python"]
 ---> Running in 9b0638a9e504
Removing intermediate container 9b0638a9e504
 ---> 3a8619e7a24c
Step 9/9 : CMD ["app1.py"]
 ---> Running in c514667b3032
Removing intermediate container c514667b3032
 ---> c44c04786606
Successfully built c44c04786606
Successfully tagged elhay/myfirstimgaeimsocoool:test1


```

now we have the image so let's run it as we learened already :) 

```
docker run -it -p 5000:5000 elhay/myfirstimgaeimsocoool:test1
 * Serving Flask app "app1" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)

```

now in docker mode 

```
docker run -it -d -p --name test 5000:5000 elhay/myfirstimgaeimsocoool:test1 

docker logs test 

```

now let's push it :) 

```
docker push elhay/myfirstimgaeimsocoool:test1
```

validate networks 
```
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
c2c695315b3a        bridge              bridge              local
a875bec5d6fd        host                host                local
ead0e804a67b        none                null                local
```

## building from base image 

By default, the stages are not named, and you refer to them by their integer number, starting with 0 for the first FROM instruction. However, you can name your stages, by adding an as <NAME> to the FROM instruction. This example improves the previous one by naming the stages and using the name in the COPY instruction. This means that even if the instructions in your Dockerfile are re-ordered later, the COPY won’t break. 
```
FROM elhay/builder as build
WORKDIR /tmp
COPY . /tmp

RUN jekyll build

FROM elhay/go-clean

COPY --from=build /tmp/_site /srv/http
```` 

```
docker build -f BuildBase -t elhay/ceango . --build-arg proxy if needed 
```
