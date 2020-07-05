>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
											## How Docker containers work in depth
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

#1. What is a Docker Container
#2. Listing and creating containers

#3. Remove and Rename Containers
#4. More tips... Start, Stop, Map Ports!

#5. Understand the Container's FileSystem


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
												#1. What is a Docker Container
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

 * Runtime of an image.
 * Containers are Temporary.
 * The Layer is Read Write.
 * WE can create as many container's as we want from an image

1. Container is nothing but a Runtime Presentation of an Image

2. Containers are temporary ( if you do any changes to container , once you destroy the container all of the changes are gone )
    If you want a permanent change you need to apply the change to the image , Probably you need to build a new image

3.  File system or R/W layer of the containers are temporary.
 ( if you destroy the container every thing that was inside of that container , that is not in the image is going to be deleted )

4. Container is basically a Layer with Read and Write Permisiions.

5. We can create as many conatiners as you want from single image. 


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
										  #2. Listing and Creating containers
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

Docker RUN Command :  Docker RUN command allows you to create containers.

Docker run command has lot of options , Just type  [ docker run  -- help ] you get lot of otions

By using docker run we can add 
        
		-- cpu limits
		-- we can put containers in detach mode 
		-- we can do DNS
		-- we can add host name 
		-- we can add IP'single
		-- We can label a container 
		-- we can add memory limits 
		-- We can assign a names to the container.
		
# To Create a container we basically need a Image.

[ docker images]

[ docker run -d ngnix:alpine ]

then if we enter [ docker ps  ]  you see your container's  which are  Up and Running


[ Docker run ]  -Basically with Run you can create a container  and  [ Docker ps ] with ( docker ps ) you can see your running conatiners


[ docker ps -a ] or [ docker ps --all ] To check  exited container's  or stopped containers   


[ docker container ls ] --> Output is Exactly the same as [ docker ps ]


Just that Docker wanted to segregate the commands for Containers and  Images for simplicity 
 

[ docker container create ngnix:alpine ]    = = [ docker run -d ngnix:alpine ]


User's have become more familiar with  ( docker ps ) rather than [ docker container ls ] and  docker run    



We see in internet these commands , as new users are used to this commands [ docker container create ngnix:alpine ] , [ docker container ls ]

But old users or all users are used to docker run , docker ps


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
												#3. Remove and Rename Containers
docker rm  <container-id> / <container-name>  

docker rm -f  <container-id> / <container-name>  

docker rename <old-name> <New-name> 
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


[ docker ps ] - will list all of the running containers  which are up and running

*1. You will always find a Container-ID   which is a unique hash or unique-id for your container. [ CONTAINER-ID ]

*2. You will always have an image , which is the image that is used to start the container. [IMAGE ]

*3. You will see CMD of that container which is defined inside the image [COMMAND ]

*4. You will see creation time  [CREATED]

*5. You will see status of container weather its Running  or Exited   [STATUS ]

*6. You will see about port , means which ports are been mapped  to particular container  [PORT]

* 7. You will see the name of the container  [NAME] 
       
	 Remember if you dont provide the name of the container , docker will automatically assign a random name for your container


# How to Remove the container

[docker rm  <container-id> / <container-name> ] 

# Remove the Container Forcibly (-f)

[ docker rm -f  <container-id> / <container-name> ] 


[ docker pull jenkins/jenkins ]

Crete a container out of this jenkins downloaded image. [ docker run -d  jenkins/jenkins ]


# what if if you forget to set a name for this container (Rename) --Rename a Running containers

In docker there is a command which allows you to change the name of the conatiner  [  docker rename [old-name] [New-name]  ]



------------------------------------
What we learnt:

1. Remove a container.
2. Forcibly Remove a container.
3. How to rename a running container 

--------------------------------------


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
												#4. More tips... Start, Stop, Map Ports!
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

# How to map ports on a container


Docker run  instruction publish, which is  going to publish and publish is going to expose a port in a container  


Docker run -d --name <name-of-container>  <image-name>

[docker run -d  --name ksathyan-jenkins jenkins/jenkins ]


Basically we can map the PORT of the container to your DOCKER HOST 

Containers are basically is an tiny operating system which is an isolated process  , if we want to acesss that service 

You need to share the the port of the container with your host and your container.but how do we do that ?

We are going to expose the port.

docker run -d --name <name-of-container>  --publish  someport-localmachine :container-port <image-name>

[ docker run -d --name ksathyan-jenkins -p 9090:8080 jenkins/jenkins ]

Now enter [ docker ps ] you can see in port sestion something different 

Port :
5000/tcp , 0.0.0.0:2000->8080/tcp

we can see an interface(0.0.0.0) which means that your [HOST] is listning in all of these interfaces  
and host is exposing the port 9090 and this port is being mapped to the container port 8080  

9090 : Port of your docker host  , 8080 : Port of your docker container  

So if you want to acess the service first you need to acess these ports (9090) and these ports  forward internally to these ports (8080)
and it will de displayed in your web browser


# We can create as many as many containers as we want from an image ,with diffrent name and diffrent port

[ docker run -d --name ksathyan-jenkins-1 -p 9091:8080 jenkins/jenkins 
 
[ docker run -d --name ksathyan-jenkins-2 -p 9092:8080 jenkins/jenkins 

[ docker run -d --name ksathyan-jenkins-3 -p 9093:8080 jenkins/jenkins 

[ docker run -d --name ksathyan-jenkins-4 -p 9094:8080 jenkins/jenkins 

[ docker run -d --name ksathyan-jenkins-5 -p 9095:8080 jenkins/jenkins 


8080 : we cannot change because this is the port in which service inside the container is running.
These port( 9091, 9092 , 9093 , 9094 , 9095 ) is the only one we should change 


[ docker ps ] -- we can see 5 active jenkin container's running with these ports exposed ( 9091 , 9092 , 9093 , 9094 , 9095 )  


[ docker stop <container-name> ] 
       
	   ==> docker stop ksathyan-jenkins-1
	   ==> docker stop ksathyan-jenkins-2
	   ==> docker stop ksathyan-jenkins-3
	   ==> docker stop ksathyan-jenkins-4
	   ==> docker stop ksathyan-jenkins-5

Docker stop is noting else but killing the service service without removing removing it

[docker ps -a ] still we have in the list in exited mode 

If we want to start the container , 

[ docker start <container-name> ] 

	   ==> docker start ksathyan-jenkins-1
	   ==> docker start ksathyan-jenkins-2
	   ==> docker start ksathyan-jenkins-3
	   ==> docker start ksathyan-jenkins-4
	   ==> docker start ksathyan-jenkins-5


------------------------------------
What we learnt:

1. Learnt How to map a port of your docker machine  to the port of docker container 
2. Learnt How to stop the running container without deleting it, its not going to be removed but its going to be in exit state
3. Learnt How to see stopped container.
4. Lerant How tp start a container using docker start.

-------------------------------------

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
												#5. Understand the Container's FileSystem
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>






>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


