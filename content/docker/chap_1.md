---
title: "Docker 101"
date: 2021-03-25T15:20:09+05:30
---




## What is a container?

* A container is a self contained sealed unit of software. 
* Contains everything to run the code.
* Includes batteries and the operating system
* A container includes
  * Code
  * Configs
  * Processes
  * Networking enough to talk to other containers
  * Dependencies
  * OS(Just enough to run the code)



![](/images/d1.png?height=400)

* So the way it works, it takes all the services that make up a linux server : Networking, storage, code, interprocess communication, the whole works and it makes a copy in the Linux kernel for each container. 

* So each container has its own little world it can't see out of and other can't see in. You might have one container on a system running Red Hat Linux, serving a database, through a virtual network to another container running Ubuntu Linux running a web server that talks to the database, and that web server might also be talking to a caching server that runs in SUSE Linux based container. 

  ![](/images/d2.png?height=400)

* ***The important part to understand is it doesn't matter which linux each container is running on, it just has to be linux.*** 

* And docker is the program that manages all of this. Sets it up, monitors it, and tears it down when it's no longer needed. 

* Docker is a client program, named Docker, it's a command you type at the terminal. 

* It's also a server program that listens for messages from that command, and manages a linux running system. 

* Docker has a program which builds container from code. It takes your code along with its dependencies and bundles it up and then seals it into a container. 

* And its a service that distributes these containers across the internet and makes it so you can find others work, and right people can find your work.

* Its also a company that makes all of these.



## Docker Flow : Images to Containers



* In Docker it all begins with an image, An image is every file that makes up just enough of the operating system to do what you need to do. 
* Traditionally, you'd install a whole operating system with everything for each application you do. With docker you pair it way down so that you have a little container with just enough of the operating system to do what you need to do, and you can have lots and lots of these efficiently on a computer. 



* To list images

  ```foo
  docker images
  ```

  ```foo
  OUTPUT : 
  
  REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
  ubuntu        latest    f643c72bc252   5 weeks ago     72.9MB
  hello-world   latest    bf756fb1ae65   12 months ago   13.3kB
  ```

  * `REPOSITORY` : Where the image came from
    `TAG` : Version number

    `IMAGE ID` : Internal docker representation of this image. 

    In any docker command i can refer to the image as `ubuntu:latest` i.e `REPOSITORY:TAG` OR by its `IMAGE ID`

* To run an image

  ```foo
  docker run -ti ubuntu:latest bash
  ```

  `t` stands for terminal, `i` stands for interactive. The above command will open a bash shell

  `docker run` command takes an image and turns it into a living container with a process 

* To exit an image type `exit` or `ctrl + D`

* To check running images/containers

  ```foo
  docker ps 
  ```

  ```foo
  OUTPUT : 
  
  CONTAINER ID   IMAGE           COMMAND   CREATED          STATUS          PORTS     NAMES
  810fba31196d   ubuntu:latest   "bash"    40 seconds ago   Up 38 seconds             lucid_noyce
  ```

  * `CONTAINER ID` : Unique container id. Different from `IMAGE ID`. Cannot be interchanged



Here's the difference between docker and virtual machines or running things on a real computer. When you're inside a container you start from an image and it doesn't change. When i make a container from the image, i don't change the image.

So if i make some changes in a running container let's just say create a file named `a.txt` and in a new parallel terminal window i run the image again(which will make a new container) than `a.txt` will not be in that container.

Even if i exit the container in which i created the file and run the image again in a new terminal window than the it will start fresh and `a.txt` will not be in that container because that will be a fresh new container.



## Docker Flow : Containers to Images



So we went from an image to a running container. When we ran that container again, we got the same thing we got the first time. And that's the whole point of images, it's they are fixed points where you know everything's good and you can always start from there. 



* Now when you've got a running container, you make changes to that container, you put files there, its very useful when you want to be able to actually save those. 

* The next step in docker flow is a stopped container. When the process exits, the container is still there. When we create a file and exit the container, it's still there. We can go back and find it. It didn't get deleted. It's just that it's currently in a stopped container. 

* To see all containers

  ```foo
  docker ps -a
  ```

* To see last created container

  ```foo
  docker ps -l
  ```

* Alright so let's say we have a stopped container that has a file we want to use for the future. We have made our changes installed our software. The next step is `docker commit` command. That takes containers and makes images out of them. 

  ![](/images/d4.png?height=400)

  It doesn't delete container, the container is still there. Now we have an image with the same content that was in that container. So `docker run` and `docker commit` are complimentary to each other. `docker run` takes images and to container and `docker commit` takes containers back to images. It doesn't overwrite the image that the container was made from. 

* To commit : making image from a container

  ```foo
  docker commit CONTAINER ID
  ```

  It will return a `sha` and make an image. To give name to that image you can do

  ```foo
  docker sha image_name
  ```

  After this you can check by running `docker images` and you will find your image there

* We can simplify the above process by doing this. It was just to demonstrate what is going on under the hood.

  ```foo
  docker commit CONTAINER_NAME IMAGE_NAME
  ```




## Running Processes in Containers



Now that we've got the Docker flow in mind let's talk about running steps in docker because that's what it's all about. 

* `docker run` starts a container by giving an image name and a process to run in that container.

* This is the main process to a container. When that process exits, the container's done. 

* If you start other processes in a container later which we'll cover, the container still exits when that main process finishes. 

* So Docker containers have one main process and they can have names. If you don't give it a name it'll make one up. 

* ```foo
  docker run --rm -ti ubuntu sleep 5
  ```

  `--rm` means when you want to run something in your container but don't wanna keep the container afterwards . This says that delete the container when it exits.

* ```foo
  docker run --rm -ti ubuntu bash -c "sleep 3; echo all done"
  ```

  Very often we want to start a container that says first run this. When it finishes, run the second thing. This pattern is extremely common for that. So in the above command container will first sleep for 3 seconds than will echo "all done".

* ```foo
  docker run -d -ti ubuntu bash
  ```

  ```foo
  OUTPUT : 
  
  039b397e383bae0be701c180ac9fc6ee37d2103f1172d94e039b6407b7b4c151
  ```

  Docker has this idea of detached containers. You can start a container running and just let it go. It'll print an identifier by which you can attach to this identifier. You don't have to use this identifier as you can use the `docker ps` command to see the name and attach the container.

* ```foo
  docker attach CONTAINER_NAME
  ```

  It'll attach you to the container which was running in background.

* To detach from the container press `Ctrl + p, Ctrl + q`. It exits you out of the container by detaching from it. 

* So say if you want to run more processes in a container because until we were only using one process in a container. We can do this by using `docker exec` command.

* So say in one terminal window you have your container running. Now open a new window and run

  ```foo
  docker exec -ti CONTAINER_NAME bash
  ```

  It'll start the same container you have in your first terminal window assuming the container name is same. You can verify it by creating a file in one window and doing the `ls` in other window as you'll see it there. 

  If you exit from one window, the other window will also exit the container. 



## Manage Containers



* Looking at a container output of something that's already finished can be very frustrating. You start up a container, it didn't work, you want to find out what went wrong. 

* `docker logs ` command is the key for this. Docker keeps the output of the container available. It keeps it around as long as you keep the container around. 

* You can use `docker logs CONTAINER_NAME` to look at what the output was. For eg. 

  ```foo
  docker run --name example -d ubuntu bash -c "llll"
  ```

  Suppose we accidentally executed a command which is wrong and it won't work. Later when we see its not running we can check what went wrong by running

  ```foo
  docker logs example
  ```

   ```foo
  OUTPUT : 
  
  bash: llll: command not found
   ```

  You can view the logs of a container as many times you want as long as it is there. 

* ***Don't let the output of your docker containers get really really huge.*** It's very convenient, being able to go back and look at it. And if you're writing tons and tons of data to the output of the process in your docker container, you can really bog down docker to the point where your whole system becomes unresponsive. 



#### Stopping and Removing Containers

* You can `kill` a running container. And when you've killed it, it goes to the stop state. And when you're done with a container in the stopped state, you remove it. 
* `docker kill CONTAINER_NAME` makes it stop. 
* `docker rm CONTAINER_NAME` makes it gone. 
* Stopped containers still exist until you remove them.



#### Resource Constraints

* One of the big features of docker is the ability to enforce on how many resources a container is going to use. 

* You can limit it to a fixed amount of memory. 

  ```foo
  docker run --memory maximum-allowed-memory IMAGE_NAME command 
  ```

* You can equally limit the CPU time

  ```foo
  docker run --cpu-shares relative to other containers
  ```

  You can limit relative, you can give this container half of the total CPU time, and other one the other half, so if one's not busy, the other can use more CPU, but they'll enforce it that they'll have equal access. 

  You can also give them hard limits 

  ```foo
  docker run --cpu-quota to limit in general
  ```

  You can say this container only gets to use 10% of the CPU even if the other 90% is idle. 

* Most of the orchestration systems require you to state limits of a particular task or a container. 



## *IMPORTANT LESSONS FROM EXPERIENCE* 

* ​	Don't let your containers fetch your dependencies when they start. If you're using something like `Node.js` and you have your node starts up when the container starts it fetches it's dependencies. Then, someday somebody's gonna remove some library from node repos and suddenly everything stops working. Make your containers include dependencies inside them, saves a lot of pain.
* Don't leave important things in unnamed stopped containers. 

