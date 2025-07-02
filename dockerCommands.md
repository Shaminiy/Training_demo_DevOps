# Docker Commands 

---

## 1. Run a Container from an Image

```sh
docker run <image_name>
```
- Tries to find the image locally; if not found, downloads from Docker Hub.

```sh
docker run -it <image_name> <command like bash>
```
- Automatically logs you into the Docker container with the specified command (e.g., `bash`). docker terminal is not visible post this, basically attach mode.

```sh
docker run -d <image_name>
```
- Runs the container in detached mode(in the background). here you can docker terminal again.
- To attach back the detached container use below :
```sh
docker attach <container_id/name>
```

---

## 2. List All Running Containers

```sh
docker ps
```
- Shows all running containers with their IDs, names, and details.

---

## 3. List All Containers (Including Stopped)

```sh
docker ps -a
```
- Shows all containers (running and stopped) with their IDs, names, and details.

---

## 4. Stop a Running Container

```sh
docker stop <container_id/name>
```
- Stops the container gracefully, allowing it to finish current tasks.

---

## 5. Remove Containers

- **Remove a stopped container:**
  ```sh
  docker rm <container_id/name>
  ```
- **Remove all stopped containers:**
  ```sh
  docker container prune
  ```
- **Remove all containers:**
  ```sh
  docker rm $(docker ps -a -q)
  ```
- **Remove all running containers:**
  ```sh
  docker rm -f $(docker ps -a -q)
  ```
- **Remove all containers (running and stopped):**
  ```sh
  docker rm -f $(docker ps -aq)
  ```
- **Remove multiple containers at once:**
  ```sh
  docker rm <container_id1> <container_id2> <container_id3>
  ```
  *(You can specify the starting digits of container IDs)*

---

## 6. Remove an Image

```sh
docker rmi <image-name>
```

## 7. List of Images

```sh
docker images
```
- Lists all local images.

```sh
docker image ls
```
- Alternative command to list images (same as `docker images`).

```sh
docker images -a
```
- Lists all images, including intermediate image layers.

```sh
docker images --filter "dangling=true"
```
- Lists only dangling (unused) images.

```sh
docker image inspect <image_name_or_id>
```
- Shows detailed information about a specific image.

```sh
docker image history <image_name_or_id>
```
- Shows the history of an image (layers and commands used to build it).

## 8. Execute a Command in a Running Container

```sh
docker exec -it <container_id/name> <command>
docker exec -it <container_id/name> cat <path to view the file>
```
- Runs a command (e.g., `bash`, `sh`, etc.) inside a running container. The `-it` flag allows interactive mode. It will not allow to type next commands, press ctrl+c to exit the comtainer , basically this is attach mode.

```sh
docker exec -it <container_id/name> bash
```
- Opens an interactive Bash shell inside the running container.

```sh
docker exec -d <container_id/name> <command>
```
- Runs a command in a running container in detached mode (the command runs in the background and does not attach your terminal).

## 9. Name a Container While Running

```sh
docker run --name <container_name> <image_name>
```
- Runs a container from the image and assigns it a custom name to container.

## 10. Tags

```sh
docker pull <imagename:version/tag> 
```
- pulls the version specified.

## 11. interactive mode
- By default, docker container does not listen to a standard input, even though console is attached. Cannot read any input from user because it doesn't have a terminal to read inputs from. It runs in a non interactive mode.
- To provide input, you must map the standard input of your host to the Docker container using -i parameter.
- To have application terminal attach to container, use -t(sudo terminal) parameter

```sh
docker run -it <image_name/id> // -it = interactive sudo terminal in the container
```

## port mapping concept.
- where Docker is installed is called Docker host or Docker engine

    how does a user access my application? application can be accessed by using Port number shown in container.

    But what IP do I use to access it from a web browser?
    2 ways : 
    1. Use the IP of the Docker container. Every Docker container gets an IP assigned by default.(Remember, this is an internal IP and is only accessible within the Docker host.)(internal ip : docker inspect the container -> network object in JSON -> IP address)
    2. Mapping a port to docker host and accessing it using the external IP
    open a brower from docker host, browser url would be https://internalIP:Portnumber would work.
    if external users want to connect, port need to map using: 

```sh
docker run -p <localhost of user/hostport>: <applicationhost/containerport> <image_name> 
```

## volume mount
- lets say you have a database mysql with lot of application data in a specific path(/var/lib/mysql) in container(mysql). Now if you remove/delete the mysql container, data along with it is deleted. So to map a directory outside the container on the docker host to a directory inside the container.
for this first create a dir outside of container(/opt/mysqlbp), then map.

```sh
docker run -v <dir path created outside of container>: <path in container> <image_name> 
```

## Inspect command

```sh
docker inspect <container id/name>
```
- shows the complete details of the container in JSON format.

## log command

```sh
docker log <container id/name>
```
- shows logs of a container.





