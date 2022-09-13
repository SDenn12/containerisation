# Containerisation

![image](https://user-images.githubusercontent.com/110126036/189627860-2ec392f7-68f7-46db-8cab-5a4fba19deda.png)

## What is Docker?

Docker is an open source containerization platform. It enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

## Difference between Virtualisation and Containerisation

Virtualization enables you to run multiple operating systems on the hardware of a single physical server, while containerization enables you to deploy multiple applications using the same operating system on a single virtual machine or server.

![image](https://user-images.githubusercontent.com/110126036/189630917-69ef18e1-e78c-4e06-8f9b-da62d5553f02.png)

## Benefeits of Containerisation and Docker?

- Portability
- Efficiency
- Agility
- Faster delivery
- Faster app startup
- Easier management
- Flexibility

## Downsides of Containerisation

All containers on a particular host machine must be designed to run on the same kind of OS. Containers based on a different OS will require a different host. 

Because the OS is shared, a security vulnerability in the OS kernel is a threat to all containers on the host machine. That being said containers are isolated from one another, you can be confident that your applications are running in their own self-contained environment. That means that even if the security of one container is compromised, other containers on the same host remain secure.  

Containerization is still a new solution with wide variances in implementation plans and skilled resources, making adoption a challenging process for some.

## What is MicroServices Architecture

A monolithic application is built as a single unified unit while a microservices architecture is a collection of smaller, independently deployable services.

Communicate using API's.

## Who is using Docker?

- Spotify 
- Uber
- Many other large companies too.

## Commands

```
docker run -d -p 80:80 nginx  # runs the nginx image on port 80 (and detached)
docker stop CONT_ID  # stops the container
docker start CONT_ID  # runs the container (from the stopped position)
docker rm CONT_ID -f # deletes the container
docker ps  # shows running containers
docker images  # lists the images
docker rmi IMG_ID  # deletes the image
```

## How to push to Dockerhub (nginx example)

### Create repository on Docker hub

![image](https://user-images.githubusercontent.com/110126036/189679597-0f39bb7e-4804-422d-ba12-5d3dca362d5d.png)

### Retrieve Nginx image

```
docker run -d -p 80:80 nginx
```

-d means it runs detached -p allows to define ports (useful for reverse proxy)


### Edit the image

```
docker exec -it CONT_ID bash
```

![image](https://user-images.githubusercontent.com/110126036/189679273-0733e419-25bd-46b9-9b59-5ed8217e61fe.png)

introduces interactive bash script where you are root user (where you can make changes to the image)

### Create your own image and push to DockerHub

```
docker commit CONT_ID sam-nginx-playaround  # creates image
docker tag IMG_ID sdennis3141/nginx_tutorial:latest  # tags the image
docker push sdennis3141/nginx_tutorial  # pushes to dockerhub
```

![image](https://user-images.githubusercontent.com/110126036/189686167-e2666c6f-68b8-4666-bff9-2690564545a8.png)

If you get access denied you can run the following command

`docker login -u "myusername" -p "mypassword" docker.io`

#### NOTE TO FIND THE IMAGE ID AND CONTAINER ID USE `docker ps -a` and `docker images`

## Editing from CLI

### Create a html file (named index.html)

```
<html>
<head>
  <title>Welcome to Sam's Website</title>

  <body>
    <h1> Hello there, I am Sam</h1>
    <h2> This website is hosted inside a Container using Docker!</h2>

  </body>
</head>
</html>
```
### Create NGINX Container
`docker run -d -p 80:80 nginx`

### Find out Container ID 

![image](https://user-images.githubusercontent.com/110126036/189878627-604495fc-06f7-441c-a1fb-726bc2849a3d.png)

#### Note that 96be7be49dcf is the container ID

### Now delete the existing file with nginx

`docker exec 96be7be49dcf rm -rf /usr/share/nginx/html/index.html`

### Replace the deleted file with the above html file

`docker cp index.html 96be7be49dcf:/usr/share/nginx/html`

### And it is finished 

![image](https://user-images.githubusercontent.com/110126036/189878911-f196c011-4c6c-4575-9d55-d0ad39dc24b8.png)

## Creating a web server using Dockerfile

### Create index.html

```
<html>
<head>
  <title>Welcome to Sam's Website</title>

  <body>
    <h1> Hello there, I am Sam</h1>
    <h2> This website is hosted inside a Container using Docker!</h2>

  </body>
</head>
</html>
```

### Create a Dockerfile

```
# Select NGINX base image
FROM nginx

# label it
LABEL MAINTAINER=sdennis@spartaglobal.com

# copy data from localhost to the container
COPY index.html /usr/share/nginx/html/

# allow required port
EXPOSE 80

# execute required command
CMD ["nginx", "-g", "daemon off;"]

```

### Build the image 

`docker build -t sdennis3141/eng122_nginx_tutorial_web_hosting:v1.0 .`

### Run the image

`docker run -d -p 80:80 sdennis3141/eng122_nginx_tutorial_web_hosting:v1.0`

### And it is finished

![image](https://user-images.githubusercontent.com/110126036/189878911-f196c011-4c6c-4575-9d55-d0ad39dc24b8.png)
