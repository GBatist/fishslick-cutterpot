# Practice

## Summary

- [Practice](#practice)
  - [Summary](#summary)
  - [Installing Docker: Choose Your Path](#installing-docker-choose-your-path)
    - [Checking Your Installation](#checking-your-installation)
  - [First Steps](#first-steps)
    - [Checking Created Containers](#checking-created-containers)
    - [Getting the Logs](#getting-the-logs)
    - [Starting and Stopping Containers](#starting-and-stopping-containers)
    - [Removing Containers](#removing-containers)
    - [Executing Commands Inside a Container](#executing-commands-inside-a-container)
  - [Creating an Image](#creating-an-image)
    - [Building the Created Image](#building-the-created-image)
    - [Searching for Image Details](#searching-for-image-details)
    - [Creating a Container](#creating-a-container)
  - [Updating the Source Code](#updating-the-source-code)
    - [Managing Tags](#managing-tags)
  - [Sharing your Image](#sharing-your-image)
    - [Repository Creation](#repository-creation)
    - [Local Image Fix-up](#local-image-fix-up)
    - [Pushing the Image](#pushing-the-image)
    - [Using the Repository Image](#using-the-repository-image)
  - [Knowing Volumes](#knowing-volumes)
    - [Volume Creation](#volume-creation)
    - [Listing Created Volumes](#listing-created-volumes)
    - [Details About the Volume](#details-about-the-volume)
    - [Attaching the Volume to the Container](#attaching-the-volume-to-the-container)
    - [Verifying Data](#verifying-data)
  - [Knowing Networks](#knowing-networks)
    - [Attaching Networks to Containers](#attaching-networks-to-containers)
  - [Moving Things to Docker Compose](#moving-things-to-docker-compose)
    - [Application Service](#application-service)
    - [Database Service](#database-service)
    - [Running the Application from the Compose File](#running-the-application-from-the-compose-file)
    - [When to use Dockerfile x Docker Compose](#when-to-use-dockerfile-x-docker-compose)

---

## Installing Docker: Choose Your Path

Ready to embark on your Docker journey? You have two paths to choose from for installation:

- [**Docker Desktop**](https://docs.docker.com/get-docker/)
  - **The Graphical Option**: This user-friendly application provides a visual interface for managing your Docker environment, simplifying tasks and offering handy features like resource visualization and easy access to Docker Hub.

  - **Integration Made Easy**: Seamless connection to Docker Hub is a breeze with Docker Desktop, making pulling containers and sharing your creations smooth and effortless.

- [**Docker Engine**](https://docs.docker.com/engine/install/)
  - **Command-Line Connoisseurs**: Experienced users preferring a CLI-driven approach can opt for Docker Engine installation. This method provides direct access to the core Docker functionalities through terminal commands.

  - **Customization Potential**: Docker Engine grants advanced users greater control and flexibility for tailoring their Docker environment to specific needs.

**My Recommendation**: For beginners and those seeking a visual and accessible experience, Docker Desktop is the way to go.

### Checking Your Installation

Once you've chosen your path and completed the installation, verifying if everything is set up is easy! Simply open your terminal and run:

```bash
docker version
```

This command will display your installed Docker version, confirming your successful entry into the world of containerization.

## First Steps

Once you have Docker installed, it is time to start the very first steps. Let's use a simple image along with this documentation that was created just to get you started with Docker. Enough talking. Let's run the following command:

```bash
docker run -d -p 8023:80 docker/getting-started
```

Let's tear down the commands contained in that line:

- `docker run`: from this instruction, we are calling the Docker engine to execute an image.
- `-d`: this indicates to Docker that the container should be executed in daemon mode, which means that the execution of the container will be performed in the background. If that was not informed, container execution will happen in the foreground in your terminal.
- `-p`: indicates to Docker which TCP ports the container will use to communicate. The syntax of this command applies something called *port forwarding*, which is a way to indicate to Docker that a bind between your local machine ports will happen with the container port; in that case, our local port `8023` when called is calling the port `80` of the container due to the bind.
- `docker/getting-started`: indicates to Docker the complete name of the name; on that part, we have the name of the user that creates the image `docker` and the name of the image itself `getting-started`.

Once you run the command, you will see the following output on your terminal:

```bash
11:59:44 ❯ docker run -d -p 8023:80 docker/getting-started
Unable to find image 'docker/getting-started:latest' locally
latest: Pulling from docker/getting-started
c158987b0551: Pull complete
1e35f6679fab: Pull complete
cb9626c74200: Pull complete
b6334b6ace34: Pull complete
f1d1c9928c82: Pull complete
9b6f639ec6ea: Pull complete
ee68d3549ec8: Pull complete
33e0cbbb4673: Pull complete
4f7e34c2de10: Pull complete
Digest: sha256:d79336f4812b6547a53e735480dde67f8f8f7071b414fbd9297609ffb989abc1
Status: Downloaded newer image for docker/getting-started:latest
3ce0f0e56fff84bccfeca1dd2b782a5d184741f8d023f556a7b065db5d9d51f6
```

### Checking Created Containers

To check if the created containers are executing, you can use two commands to check that; both gives you the same result, so feel free to use `docker container ls` or `docker ps`, in both executions, you will see something like this:

```bash
13:43:22 ❯ docker container ls
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                  NAMES
3ce0f0e56fff   docker/getting-started   "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:8023->80/tcp   sad_jang
13:47:47 ❯ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                  NAMES
3ce0f0e56fff   docker/getting-started   "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:8023->80/tcp   sad_jang
```

### Getting the Logs

In case you need to take a look at the logs of a container, you will need to have the `Container ID` or the `Name` - which you already saw when you executed the command to check created containers. Be aware that those two are necessary to execute a reasonable number of Docker commands. So, with that information in hand, you can use the command `docker logs -f <Container ID/Name>`

The important point of this command is the usage of the parameter `-f`, this parameter allows you to see the log file in real time, which means that for each new input on that file you will see that happening in your terminal.

After executing that command, your output should look like the following:

```bash
13:47:56 ❯ docker logs -f 3ce0f0e56fff
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/01/16 16:43:21 [notice] 1#1: using the "epoll" event method
2024/01/16 16:43:21 [notice] 1#1: nginx/1.23.3
2024/01/16 16:43:21 [notice] 1#1: built by gcc 12.2.1 20220924 (Alpine 12.2.1_git20220924-r4)
2024/01/16 16:43:21 [notice] 1#1: OS: Linux 5.15.133.1-microsoft-standard-WSL2
2024/01/16 16:43:21 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/01/16 16:43:21 [notice] 1#1: start worker processes
2024/01/16 16:43:21 [notice] 1#1: start worker process 30
2024/01/16 16:43:21 [notice] 1#1: start worker process 31
2024/01/16 16:43:21 [notice] 1#1: start worker process 32
2024/01/16 16:43:21 [notice] 1#1: start worker process 33
172.17.0.1 - - [16/Jan/2024:16:55:05 +0000] "GET / HTTP/1.1" 200 8702 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [16/Jan/2024:16:55:06 +0000] "GET /tutorial/ HTTP/1.1" 200 14807 "http://localhost:8023/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [16/Jan/2024:16:55:06 +0000] "GET /tutorial/tutorial-in-dashboard.png HTTP/1.1" 200 109800 "http://localhost:8023/tutorial/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [16/Jan/2024:16:55:06 +0000] "GET /fonts/hinted-Geomanist-Book.ttf HTTP/1.1" 200 73568 "http://localhost:8023/css/styles.css" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [16/Jan/2024:16:55:06 +0000] "GET /assets/fonts/specimen/MaterialIcons-Regular.woff2 HTTP/1.1" 200 44300 "http://localhost:8023/assets/fonts/material-icons.css" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [16/Jan/2024:16:55:06 +0000] "GET /assets/fonts/specimen/FontAwesome.woff2 HTTP/1.1" 200 77160 "http://localhost:8023/assets/fonts/font-awesome.css" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" "-"
```

### Starting and Stopping Containers

In case you need to start or stop a container, the only difference between those actions (as you might suspect) is the keyword you are going to use, for example:

- To stop the container we have created, we can use the command `docker container stop <Container ID/Name>`
- To start the container, we can use the command `docker container start <Container ID/Name>`

To check the status of the containers after the execution of the commands above, you can use the command `docker ps -a`, the `-a` parameter indicates to Docker that it should show all the containers and not only the active ones. Running this command, your output should look like this:

```bash
14:01:40 ❯ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                          PORTS     NAMES
3ce0f0e56fff   docker/getting-started   "/docker-entrypoint.…"   18 minutes ago   Exited (0) About a minute ago             sad_jang
```

### Removing Containers

In case you need to, for some reason, remove a container, you can do that by running the command `docker rm <Container ID/Name>`, but there are some important things to mention, which are:

If you have a running container, that command is not going to work unless you:

- Stop the running container first
- Or use the parameter `-f`, like in many command-line tools, this parameter tells Docker CLI that you are going to force the execution of this command.

### Executing Commands Inside a Container

If you need to execute any kind of command inside your container, you can do that through the command `docker container exec` along with some other complements. Imagine that you will want to execute a shell on the container that we have created. The command that you will use should be something like this: `docker container exec -it <Container ID/Name> /bin/sh`, let's give more context of what is going on:

- `docker container exec`: as it was mentioned, is the core of the command that Docker CLI understands that you want to execute command inside the container.
- `-i`: indicates to the CLI that you require interactivity with the opened terminal instance
- `-t`: indicates to the CLI that a terminal session must be created
NOTE: For a deeper explanation of the parameters `-i` and `-t` run the command `docker container exec —help`
- `/bin/sh`: is the command we are requiring to be executed; in this case, we require a shell that is in that path

As an example of what your terminal may look like while executing a command inside a container, take a look at the following:

```bash
14:14:37 ❯ docker container exec -it 1e201f90c678 /bin/sh
/ # whoami
root
```

**IMPORTANT**: The `exec` command is based on the container's reality, which means that for something to be able to be executed, it must exist as part of the container.

**IMPORTANT 2**: To get back to your terminal, press the combination `CTRL+D`.

## Creating an Image

To create our image, we are going to use the [following application](https://github.com/docker/getting-started-app), which is a simple to-do list application, as you can see:

```bash
14:38:14 ❯ ls

        Directory: C:\Workspace\studies\docker\getting-started-app


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        16/01/2024  14:35 PM                  spec
d----        16/01/2024  14:35 PM                  src
-a---        16/01/2024  14:35 PM            681   package.json
-a---        16/01/2024  14:35 PM            273   README.md
-a---        16/01/2024  14:35 PM         150541   yarn.lock
```

To create our image for this application, we need to create a file called `Dockerfile`. This file is responsible for having all the base instructions for our image to be built and host our application.

Now, enough talk. Let's create our Dockerfile in the root of our application by using the command `touch Dockerfile`, the content of the file should be the same as this:

```bash
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

A lot of instructions were defined; let's take a look at each one of them:

- `FROM node:18-alpine` indicates which base image we are going to use to build our own image
- `WORKDIR /app` creates a directory called `app` and set this location as the current location
- `COPY . .` performs the copy of the content where the Dockerfile is located to the current location inside the image, which means that the content of `C:\Workspace\studies\docker\getting-started-app` will be copied to `/app`
- `RUN yarn install --production` executes the yarn command like you would do on your own terminal to interact with the application
- `CMD ["node", "src/index.js"]` like the `run` command, it is going to execute the node command on the terminal
  - the difference between `RUN` and `CMD` is:
    - `RUN` is triggered in the image build process
    - `CMD` is triggered in the image execution process
- `EXPOSE 3000` indicates which TCP port will be exposed for communication, in this case the port `3000`

### Building the Created Image

With the Dockerfile created and our image ready, now it is time to build that image. On this step, we are going to tell Docker to read all the steps described in our Dockerfile, and execute them to further create our container.

We do that by executing the command `docker build -t getting-started .`, as in the majority of commands, the part `docker build` is the core of the command, so let's take a look at the others:

- `-t getting-started` by using this parameter, we are tagging our image; the tag in this case will be `getting-started`.
- `.` that indicates that the Dockerfile is at the same level of where we are executing the build command; if that is not the case, you will need to specify the whole path to the Dockerfile in that part.

After executing the build command, your terminal should look like this:

```bash
15:10:49 ❯ docker build -t getting-started .
[+] Building 15.0s (12/12) FINISHED                                                                                        docker:default
=> [internal] load build definition from Dockerfile                                                                                 0.0s
=> => transferring dockerfile: 190B                                                                                                 0.0s
=> [internal] load .dockerignore                                                                                                    0.0s
=> => transferring context: 2B                                                                                                      0.0s
=> resolve image config for docker.io/docker/dockerfile:1                                                                           2.5s
=> [auth] docker/dockerfile:pull token for registry-1.docker.io                                                                     0.0s
=> CACHED docker-image://docker.io/docker/dockerfile:1@sha256:ac85f380a63b13dfcefa89046420e1781752bab202122f8f50032edf31be0021      0.0s
=> [internal] load metadata for docker.io/library/node:18-alpine                                                                    0.0s
=> [1/4] FROM docker.io/library/node:18-alpine                                                                                      0.0s
=> [internal] load build context                                                                                                    0.2s
=> => transferring context: 6.50MB                                                                                                  0.2s
=> CACHED [2/4] WORKDIR /app                                                                                                        0.0s
=> [3/4] COPY . .                                                                                                                   0.1s
=> [4/4] RUN yarn install --production                                                                                             11.0s
=> exporting to image                                                                                                               0.8s
=> => exporting layers                                                                                                              0.8s
=> => writing image sha256:52b0bc483666175fa71fe1ad896801b5b1b17a6d2b5ff3de4b2ad90cfeaa9dad                                         0.0s
=> => naming to docker.io/library/getting-started                                                                                   0.0s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

### Searching for Image Details

To list all the built images we have so far, we can use the command `docker image ls`, which will produce something like this on our terminal

```bash
15:18:02 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED         SIZE
getting-started                            latest       52b0bc483666   7 minutes ago   223MB
```

If we would like to see more details about the image, we can execute the command `docker image inspect <Container ID/Name>`, it should produce something like this on the tour terminal - the same works for containers and networks

```bash
15:20:50 ❯ docker image inspect 52b0bc483666
[
{
"Id": "sha256:52b0bc483666175fa71fe1ad896801b5b1b17a6d2b5ff3de4b2ad90cfeaa9dad",
"RepoTags": [
"getting-started:latest"
],
"RepoDigests": [],
"Parent": "",
"Comment": "buildkit.dockerfile.v0",
"Created": "2024-01-16T18:11:30.345887078Z",
"Container": "",
"ContainerConfig": {
"Hostname": "",
"Domainname": "",
"User": "",
"AttachStdin": false,
"AttachStdout": false,
"AttachStderr": false,
"Tty": false,
"OpenStdin": false,
"StdinOnce": false,
"Env": null,
"Cmd": null,
"Image": "",
"Volumes": null,
"WorkingDir": "",
"Entrypoint": null,
"OnBuild": null,
"Labels": null
},
"DockerVersion": "",
"Author": "",
"Config": {
"Hostname": "",
"Domainname": "",
"User": "",
"AttachStdin": false,
"AttachStdout": false,
"AttachStderr": false,
"ExposedPorts": {
"3000/tcp": {}
},
"Tty": false,
"OpenStdin": false,
"StdinOnce": false,
"Env": [
"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
"NODE_VERSION=18.19.0",
"YARN_VERSION=1.22.19"
],
"Cmd": [
"node",
"src/index.js"
],
"ArgsEscaped": true,
"Image": "",
"Volumes": null,
"WorkingDir": "/app",
"Entrypoint": [
"docker-entrypoint.sh"
],
"OnBuild": null,
"Labels": null
},
"Architecture": "amd64",
"Os": "linux",
"Size": 223474556,
"VirtualSize": 223474556,
"GraphDriver": {
"Data": {
"LowerDir": "/var/lib/docker/overlay2/7hawsg8wxxvpwqpcvhrbf1u1e/diff:/var/lib/docker/overlay2/ws7g989rn6q63qriw9ejuwc3x/diff:/var/lib/docker/overlay2/cb91ee4fbf99c8fffe3fa5174f1cfeb2ab56f3b00f67c84ec190417ab55c5940/diff:/var/lib/docker/overlay2/3b7b6c2245af1a56d889cc9a09ac5fe33d1879caa44576ab7d1578fcb5256e8c/diff:/var/lib/docker/overlay2/54b18a2aa88a60b43622d70325e672b25a4a6a642f5c94c14ce660367555fb47/diff:/var/lib/docker/overlay2/5904e5caf37d4d03c41b8741db566efd8c86af1ea8541e7aca3d6fbbd3713f30/diff",
"MergedDir": "/var/lib/docker/overlay2/f7md7zavm7exr20jlael1kpwj/merged",
"UpperDir": "/var/lib/docker/overlay2/f7md7zavm7exr20jlael1kpwj/diff",
"WorkDir": "/var/lib/docker/overlay2/f7md7zavm7exr20jlael1kpwj/work"
},
"Name": "overlay2"
},
"RootFS": {
"Type": "layers",
"Layers": [
"sha256:5af4f8f59b764c64c6def53f52ada809fe38d528441d08d01c206dfb3fc3b691",
"sha256:62bb8375428b10bf660febe0b9819175d19de272f45ddbbc5db8db9426913256",
"sha256:aec6514001fe82bb9a720e47e50208562f8db245910ba9ff862441dd7f846654",
"sha256:0ae13a02eeea43822675d6efa7784f49c7a1c472882d2ea98b9e5ba9dbc494d4",
"sha256:c8a0db1d4b9b0141266bb07fc08523f4cb6b4e24653b67d56fdc5c8366f905d2",
"sha256:404393ccc5406e36a973c9c3ba531cfc9794a1ccdc7ee4c12e59275900f202eb",
"sha256:99aebb8b75aab4911411f6c2362a67b09eec83eb02b48c0735a4dacff7073370"
]
},
"Metadata": {
"LastTagTime": "2024-01-16T18:11:31.203880319Z"
}
}
]
```

### Creating a Container

With our image created and built, it is time to create a container. To do that, we need to execute the following command: `docker run -d -p 3000:3000 getting-started:latest` that is going to create the container, as we did in the first steps section.

After that is executed, let's take a look at the list of active containers to confirm that our container is up and running

```bash
15:40:41 ❯ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                    NAMES
0d97d970b3d1   getting-started:latest   "docker-entrypoint.s…"   4 seconds ago   Up 3 seconds   0.0.0.0:3000->3000/tcp   hopeful_swanson
```

## Updating the Source Code

Okay, now we have an image created, built, and a container (which is impressive so far), but let's take a moment to think about one specific thing: "What happens if our source code is updated?"

Well, that is not a huge problem; let's work on that scenario so you understand better what should be done. First, let's make some changes to our source code, like this:

```bash
15:50:11 ❯ git diff src/static/js/app.js
diff --git a/src/static/js/app.js b/src/static/js/app.js
index 4ec962c..1fb76fd 100644
--- a/src/static/js/app.js
+++ b/src/static/js/app.js
@@ -53,7 +53,7 @@ function TodoListCard() {
         <React.Fragment>
             <AddItemForm onNewItem={onNewItem} />
             {items.length === 0 && (
-                <p className="text-center">No items yet! Add one above!</p>
+                <p className="text-center">You have no todo items yet! Add one above!</p>
             )}
             {items.map(item => (
                 <ItemDisplay
```

All we have to do is to run a new `docker build` command, but we need to follow the good practices to make sure we do not get our infrastructure messy. The good practices say that for each new build, we must indicate that on the image tag, so what we should do is run the command `docker build -t getting-started:2.0 .`

Let's take a look at our image list:

```bash
15:56:12 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED              SIZE
getting-started                            2.0          1194c0822966   About a minute ago   223MB
getting-started                            latest       52b0bc483666   44 minutes ago       223MB
```

### Managing Tags

You can notice that on the first time we performed the build command, we did not create a tag correctly and our image ended up receiving the default tag that Docker uses. Docker uses `latest` as the tag for the image when you do not inform anything.

We can, once again following the best practices, run the command `docker image tag getting-started:latest getting-started:1.0`, which will change the tag of the image that was tagged as `latest` to the correct that should be `1.0`.

Let's take a look at the images list:

```bash
15:59:54 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED          SIZE
getting-started                            2.0          1194c0822966   5 minutes ago    223MB
getting-started                            1.0          52b0bc483666   48 minutes ago   223MB
getting-started                            latest       52b0bc483666   48 minutes ago   223MB
```

Cool, we changed the tag name... but as you can see, Docker still references the `latest` as the image `1.0`. To fix that, we need to run the tag command once again, but now we are focusing on solving this reference.

We need to specify to Docker that now the`latest` is the same as `2.0`, and the command to do that should look like this `docker image tag getting-started:2.0 getting-started:latest`. After executing this command, we can confirm that if using the `2.0` or the `latest` tag, Docker is using the same image:

```bash
16:01:49 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED          SIZE
getting-started                            2.0          1194c0822966   7 minutes ago    223MB
getting-started                            latest       1194c0822966   7 minutes ago    223MB
getting-started                            1.0          52b0bc483666   50 minutes ago   223MB
```

## Sharing your Image

Now we know how to create images and build containers, but we are only doing this locally. We need to use something called `regitry`, which, in a quick (and not too deep) definition, is a host where you have a repository that allows you to push your images and make them sharable for people to use your images on their own desire.

The most famous (and common) is [Docker Hub](https://hub.docker.com/), on this section, we are going to see how to use Docker Hub to push and share our images.

### Repository Creation

After you are finished with the login process, go to the repositories list, and let's proceed to your repository creation. You can do that by clicking on the `Create Repositorty` button, as shown in the image below:

![repository list](/images/docker_hub_repo_list.png)

After clicking on that button on the next screen is time to configure things like the name of your repository and the visibility of it. On this case we are going to name the repo as `getting-started` and setting the visibility as `public` - if you are using the free plan you can only have one private repo.

![repository creation](/images/docker_hub_repo_create.png)

After hitting the `create` button, you will be taken to the general page of your repository, which should be empty at this moment

![clean repository](/images/docker_hub_clean_repo.png)

As you already noticed, there is a command in the top right corner of your screen showing you how to push images from your machine to this new repo.

### Local Image Fix-up

Taking a look at our local images, we can see that they are not named in a way that the push instruction is not going to work:

```bash
09:33:52 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED         SIZE
getting-started                            2.0          1194c0822966   18 hours ago    223MB
getting-started                            latest       1194c0822966   18 hours ago    223MB
getting-started                            1.0          52b0bc483666   18 hours ago    223MB
```

The way we are going to fix that is by creating a new image with the right name based on the `latest` image. To do that, we are going to use the command `docker image tag getting-started:latest gcabatista/getting-started:latest`, which now gives us an image with the correct name and references the most up-to-date image:

```bash
09:38:30 ❯ docker image ls
REPOSITORY                                 TAG          IMAGE ID       CREATED         SIZE
gcabatista/getting-started                 latest       1194c0822966   18 hours ago    223MB
getting-started                            2.0          1194c0822966   18 hours ago    223MB
getting-started                            latest       1194c0822966   18 hours ago    223MB
getting-started                            1.0          52b0bc483666   18 hours ago    223MB
```

### Pushing the Image

Now that all is set, it is time to push our image using the command `docker push gcabatista/getting-started:latest`, your output after running this command should look like this:

```bash
09:41:18 ❯ docker push gcabatista/getting-started:latest
The push refers to repository [docker.io/gcabatista/getting-started]
3050fe2245c4: Pushed
da5e2f13d667: Pushed
c8a0db1d4b9b: Pushed
0ae13a02eeea: Mounted from library/node
aec6514001fe: Mounted from library/node
62bb8375428b: Mounted from library/node
5af4f8f59b76: Mounted from library/node
latest: digest: sha256:cfabea189cb3e8282c6defa26f4cf1f72ddfeecb495392980c1b80e4f853e109 size: 1787caso
```

If you are receiving and error like `denied: requested access to the resource is denied`, that means you have not performed the login on Docker Hub using the CLI already.

You can solve that using two approaches:

- via CLI, running the command `docker login -u <your dockerhub user>` and, when the password was requested, informing the access token you have created on the Docker Hub website on `My Account >> Security >> Access Token`

- via Docker Desktop, by clicking on the login button.

After the login is done, try the push command again.

Once you push succeeded, you will see that your repository looks like this:

![pushed image](/images/docker_hub_pushed_image.png)

### Using the Repository Image

If you do not have this image downloaded yet, and you do not want to run it yet, but want to download it, you can use the command `docker pull gcabatista/getting-started:latest`

Now if you want to download and use an image from your registry repository, you can do that by using the following command `docker run -d -p 3000:3000 gcabatista/getting-started:latest`, which will result in the following container:

```bash
10:06:47 ❯ docker ps
CONTAINER ID   IMAGE                               COMMAND                  CREATED          STATUS          PORTS                    NAMES
15703cbaae0b   gcabatista/getting-started:latest   "docker-entrypoint.s…"   14 seconds ago   Up 13 seconds   0.0.0.0:3000->3000/tcp   beautiful_hellman
```

## Knowing Volumes

Volumes are the way that we persist data from containers. As you have already seen in the theory section, Docker has a read/write layer, but once the container is not being executed, all the data is gone.

A way to solve that (and follow the best practices) is to use volumes, which allows you to persist data. There are two ways to use volumes: the volume creation itself and the directory binding way (which is not commonly recommended to be used anymore).

The community prefers to use volumes instead of directory binding because, this way, you let Docker handle all the necessary managements around the volumes.

### Volume Creation

Let's create a volume that is going to be used to store the data from a database. We can do that by using the command `docker volume create todo-db`, this is a very straight-forward command where you indicate that you want a volume creation with the name `todo-db`.

### Listing Created Volumes

As we have seen so far, to list the created volumes, we can use the `ls` command, the structure of the command that we are going to execute is `docker volume ls`. That is going to give you the following output:

```bash
10:25:24 ❯ docker volume ls
DRIVER    VOLUME NAME
local     todo-db
```

### Details About the Volume

In the same way to see more details about the volume we can use the `inspect` command, the structure of the command that we are going to execute is `docker volume inspect todo-db`. That is going to give you the following output:

```bash
10:27:07 ❯ docker volume inspect todo-db
[
    {
        "CreatedAt": "2024-01-17T13:25:24Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```

### Attaching the Volume to the Container

Awesome! Now that we have our volume created, it is time to tell Docker that we are going to use that volume in a container. To do that, we are going to modify a little bit the `docker run` command, and this time we are going to use the parameter `-v`.

To use the `-v` parameter, we need to indicate the volume name and where in the container this volume will be mounted. In our case, we need to mount our volume inside `/etc/tools`, so the parameter value will be `todo-db:/etc/todos`.

Perfect. Now that you know how the `-v` parameter structure works, your `docker run` command should look like this `docker run -d -p 3000:3000 -v todo-db:/etc/todos gcabatista/getting-started:latest`.

Once you execute that command, to confirm that the volume is being used by the container, we can `inspect` the container and take a look at the `Mounts` section to confirm that:

```bash
10:38:52 ❯ docker container inspect 54e8da4c1997
[
    {
        "Id": "54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f",
        "Created": "2024-01-17T13:36:56.682269131Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "node",
            "src/index.js"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 4101,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-01-17T13:36:56.966447638Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:1194c082296646e5132d8328d2fbb4d50b1d18537a913aa34bdace083a862393",
        "ResolvConfPath": "/var/lib/docker/containers/54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f/hostname",
        "HostsPath": "/var/lib/docker/containers/54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f/hosts",
        "LogPath": "/var/lib/docker/containers/54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f/54e8da4c1997ccd8a0d4bba6171446dcdaa4f6078d9e542eeb52ee9d167a188f-json.log",
        "Name": "/kind_davinci",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "todo-db:/etc/todos"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "3000/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "3000"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                22,
                138
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/83941cee61a616e23927a5fc18fcda9088a83a300e2216a751aa34808d5f9764-init/diff:/var/lib/docker/overlay2/t9hg24nokxxlov2ywjzz5hmuq/diff:/var/lib/docker/overlay2/n0crohhukcypzqqin3a119nt1/diff:/var/lib/docker/overlay2/ws7g989rn6q63qriw9ejuwc3x/diff:/var/lib/docker/overlay2/cb91ee4fbf99c8fffe3fa5174f1cfeb2ab56f3b00f67c84ec190417ab55c5940/diff:/var/lib/docker/overlay2/3b7b6c2245af1a56d889cc9a09ac5fe33d1879caa44576ab7d1578fcb5256e8c/diff:/var/lib/docker/overlay2/54b18a2aa88a60b43622d70325e672b25a4a6a642f5c94c14ce660367555fb47/diff:/var/lib/docker/overlay2/5904e5caf37d4d03c41b8741db566efd8c86af1ea8541e7aca3d6fbbd3713f30/diff",
                "MergedDir": "/var/lib/docker/overlay2/83941cee61a616e23927a5fc18fcda9088a83a300e2216a751aa34808d5f9764/merged",
                "UpperDir": "/var/lib/docker/overlay2/83941cee61a616e23927a5fc18fcda9088a83a300e2216a751aa34808d5f9764/diff",
                "WorkDir": "/var/lib/docker/overlay2/83941cee61a616e23927a5fc18fcda9088a83a300e2216a751aa34808d5f9764/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "todo-db",
                "Source": "/var/lib/docker/volumes/todo-db/_data",
                "Destination": "/etc/todos",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "54e8da4c1997",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3000/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NODE_VERSION=18.19.0",
                "YARN_VERSION=1.22.19"
            ],
            "Cmd": [
                "node",
                "src/index.js"
            ],
            "Image": "gcabatista/getting-started:latest",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "ab63ac7bb9d7ec006decb53aa5293841b54230b4ed53bcdab35d865511134308",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "3000/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "3000"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/ab63ac7bb9d7",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "36a71eebac61a2aab814033dcb48e15e722c3acfa042d473fd8bcd413ffa64de",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "NetworkID": "8d9fb40b1ab87495ae3ac7989155d919e3d80a5c1e570a7249c303155d60327b",
                    "EndpointID": "36a71eebac61a2aab814033dcb48e15e722c3acfa042d473fd8bcd413ffa64de",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DriverOpts": null
                }
            }
        }
    }
]
```

### Verifying Data

With the container running, go to the application and create some data, and after that, delete the container. If everything is working as expected once you run the command `docker run -d -p 3000:3000 -v todo-db:/etc/todos gcabatista/getting-started:latest`, your data should be presented on the application. The reason behind that is that the data is not being stored inside the container anymore.

## Knowing Networks

Networks on Docker are a way to group container in a single segment, which means that insida a network all of the containers that are part of it can communicate to each other.

Let's create a networking named `todo-app` using the command `docker network create todo-app`, and after that let's take a look on our networks list:

```bash
10:55:51 ❯ docker network ls
NETWORK ID     NAME       DRIVER    SCOPE
8d9fb40b1ab8   bridge     bridge    local
d5f3aec9f4d5   host       host      local
fbca4a30db9a   none       null      local
83c9cd8ae376   todo-app   bridge    local
```

As you can see, the `todo-app` network uses the bridge driver, which is the default driver for network creation. To read more about the differences between the drivers, please take a look here at [this explanation](https://docs.docker.com/network/drivers/).

Let's inspect the created network to see more information about it:

```bash
10:55:52 ❯ docker network inspect 83c9cd8ae376
[
    {
        "Name": "todo-app",
        "Id": "83c9cd8ae376d2f96dbb5f7b80b9c51b050c2e84c89cf0a6f61a9d8d31c3e714",
        "Created": "2024-01-17T13:55:22.998970942Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

### Attaching Networks to Containers

To give you a better example of how networks work, we are going to create a MYSQL container to integrate with our previous application container.

Moving forward, now that we have a network created, it is time to use it! And as you might be imagining right now, we are going to do that by once again modifying the `docker run` command. This time we are going to use two parameters, which are `--network` and `--network-alias`.

The parameter `--network` indicates the name of the network we are going to use on the container. The parameter `--network-alias` is the one that will make our container visible to all the members of the same network.

Sweet, so our `docker run` command will look like this: `docker run -d -v mysql-todo-db:/var/lib/mysql --network todo-app --network-alias mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:8.0`

If we inspect this new container and take a look at the `Network` section, we can confirm that the network is attached to the container:

```bash
11:24:19 ❯ docker container inspect 0b62a4f18a72
[
    {
        "Id": "0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e",
        "Created": "2024-01-17T14:24:13.954133118Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 9870,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-01-17T14:24:14.327202629Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:ba048db12589d011b77ab3ea33b7010d0b549214c8baa5fd885e4af84887928f",
        "ResolvConfPath": "/var/lib/docker/containers/0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e/hostname",
        "HostsPath": "/var/lib/docker/containers/0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e/hosts",
        "LogPath": "/var/lib/docker/containers/0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e/0b62a4f18a727e91eb6de5aa07efc63871023ab783c333247183c2b4c3a0f09e-json.log",
        "Name": "/stoic_panini",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "mysql-todo-db:/var/lib/mysql"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "todo-app",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                22,
                138
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f28690605c3fc4f00cc40240a43131c93d791f16834480e8bda1fd893050636f-init/diff:/var/lib/docker/overlay2/6d8081eff9c6976af2659348c4ff084cf086c7be3510b9eae9af3a47a33f6397/diff:/var/lib/docker/overlay2/dca9c7660e1cb3d6fe57266ef4ded40e15cda9ef547a00122b4f9f489bc6cab9/diff:/var/lib/docker/overlay2/c32421697d9c2dafd9d6783a3b4b15c5cc7e7753d3487f129938b4a5d3343701/diff:/var/lib/docker/overlay2/34ca1fc4709211576c353ae50e6f6c8a764b2cfb1f5a30d115b6c20233073ba9/diff:/var/lib/docker/overlay2/89f005155844fd1179341e6a916337a4a22465efe00278b5c70d17cc40259a7b/diff:/var/lib/docker/overlay2/2a3cc9c3c8f270fe314076805dfca51a21b8142bb86c51f9fddb2e145756368f/diff:/var/lib/docker/overlay2/102e34c9cb148f5cb4c016c9c9672d0982406ef8c554766b6ce310a702865321/diff:/var/lib/docker/overlay2/4980d0ee26c596aa7ab5ea9f446e5ebc3a71b8108e69f4064c4719d00f5332c6/diff:/var/lib/docker/overlay2/403414a13ebb306c48c41bff7776ec50d244f0f8e74fbcd369913accc2bf900f/diff:/var/lib/docker/overlay2/f493d7af0cf9cc926024295350ee7a58ab9e69d3165465d57e8a986c8b7ce115/diff:/var/lib/docker/overlay2/1d397b70ee47ef32b19cd86f736af5a29d95c2938ecad89f526adf314e591f2a/diff",
                "MergedDir": "/var/lib/docker/overlay2/f28690605c3fc4f00cc40240a43131c93d791f16834480e8bda1fd893050636f/merged",
                "UpperDir": "/var/lib/docker/overlay2/f28690605c3fc4f00cc40240a43131c93d791f16834480e8bda1fd893050636f/diff",
                "WorkDir": "/var/lib/docker/overlay2/f28690605c3fc4f00cc40240a43131c93d791f16834480e8bda1fd893050636f/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "mysql-todo-db",
                "Source": "/var/lib/docker/volumes/mysql-todo-db/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "0b62a4f18a72",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "MYSQL_ROOT_PASSWORD=secret",
                "MYSQL_DATABASE=todos",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.16",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.35-1.el8",
                "MYSQL_SHELL_VERSION=8.0.35-1.el8"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "mysql:8.0",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "ff0b13fabb5f2ea6b06b0d39c5d9ea35354d2a7088cb61ff605f087ae166f604",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "3306/tcp": null,
                "33060/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/ff0b13fabb5f",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "todo-app": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "mysql",
                        "0b62a4f18a72"
                    ],
                    "MacAddress": "02:42:ac:12:00:02",
                    "NetworkID": "83c9cd8ae376d2f96dbb5f7b80b9c51b050c2e84c89cf0a6f61a9d8d31c3e714",
                    "EndpointID": "8e28d2738b4c0d59660f2be75a54d329c376a83b512a07ae04fdd7b704b0f18e",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DriverOpts": null
                }
            }
        }
    }
]
```

To connect our application to the same network using the same logic, you will need to add the parameters `--network` and `--network-alias` to the `docker run` command.

Et voilà! Your application is on the same network as the database, and they are communicating with each other.

## Moving Things to Docker Compose

Now that we are done with the implementation of our infrastructure, it is time to polish the way we are working a little bit. So far, we have been inputting commands in-line while using the `docker run` command, but the best practices say that we should have been doing that inside a file called `compose.yaml`.

A quick definition of what the compose file by Docker is:

> *[Docker Compose is a tool that helps you define and share multi-container applications. With Compose, you can create a YAML file to define the services and with a single command, you can spin everything up or tear it all down.](https://docs.docker.com/get-started/08_using_compose/)*

Let's fix that up! First, we need to create a file named `compose.yaml` at the root of our application folder, so the structure of the folder should look like this:

```bash
13:39:13 ❯ ls

        Directory: C:\Workspace\studies\docker\getting-started-app


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        17/01/2024  11:48 AM                  node_modules
d----        16/01/2024  14:35 PM                  spec
d----        16/01/2024  14:35 PM                  src
-a---        19/01/2024  16:53 PM            514   compose.yaml
-a---        17/01/2024  11:27 AM            241   Dockerfile
-a---        16/01/2024  14:35 PM            681   package.json
-a---        16/01/2024  14:35 PM            273   README.md
-a---        16/01/2024  14:35 PM         150541   yarn.lock
```

### Application Service

On this file, we are going to have a group called `services` that groups all the containers that will be produced once you execute the compose file. As we did in our Dockerfile, we start from the image definition:

```bash
services:
  app:
    image: node:18-alpine
```

Moving forward, we are going to execute commands related to our application:

```bash
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
```

Then we can expose the necessary ports that our application is going to need:

```bash
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
```

Now we are going to create the working directory for our application and create the necessary volume to use it:

```bash
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
```

Last but not least, let's create all the necessary environment variables:

```bash
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
```

### Database Service

As the previous service section explains the idea of how a service should be written inside a compose file, for our database service, I will show here the complete configuration because there is no need to repeat the same explanation level.

```bash
services:
  app:
    # The app service definition
  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

The only addition here is the `volume` group, which is different from `docker run`, to get a volume created, you need to define in a top-level view the volume that is specified in the service mount point.

### Running the Application from the Compose File

With everything set, it is time to run our compose file and see if both of our containers will be created. We can do that by running the following command `docker compose up -d`, you should have the following output on your terminal:

```bash
14:20:36 ❯ docker compose up -d
[+] Running 17/17
 ✔ mysql 11 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                             32.8s
   ✔ 558b7d69a2e5 Pull complete                                                                                                      5.5s
   ✔ c7e714ea5470 Pull complete                                                                                                      0.6s
   ✔ 508ada0981f6 Pull complete                                                                                                      1.5s
   ✔ 1ae11d1b9d69 Pull complete                                                                                                      5.0s
   ✔ 3c9be74ddc84 Pull complete                                                                                                      2.6s
   ✔ d4a59c718252 Pull complete                                                                                                      3.9s
   ✔ ac42afee6c9c Pull complete                                                                                                     11.4s
   ✔ dcf1f586b2e2 Pull complete                                                                                                      5.8s
   ✔ 210e58b61fa3 Pull complete                                                                                                     13.4s
   ✔ d9719ad703de Pull complete                                                                                                      6.9s
   ✔ 1ef92b20c8ec Pull complete                                                                                                      8.2s
 ✔ app 4 layers [⣿⣿⣿⣿]      0B/0B      Pulled                                                                                        3.9s
   ✔ 661ff4d9561e Already exists                                                                                                     0.0s
   ✔ 0f158788f409 Already exists                                                                                                     0.0s
   ✔ f028dff98271 Already exists                                                                                                     0.0s
   ✔ 18f25c33705d Already exists                                                                                                     0.0s
[+] Running 4/4
 ✔ Network getting-started-app_default           Created                                                                             0.5s
 ✔ Volume "getting-started-app_todo-mysql-data"  Created                                                                             0.0s
 ✔ Container getting-started-app-app-1           Started                                                                             1.1s
 ✔ Container getting-started-app-mysql-1         Started                                                                             1.1s

```

And to make sure that both containers are on the same network and communicating with each other, we can take a look at the logs. We can do that using the command `docker compose logs -f`, as we can see everything is working as expected:

```bash
14:22:09 ❯ docker compose logs -f
app-1    | yarn install v1.22.19
app-1    | [1/4] Resolving packages...
app-1    | success Already up-to-date.
app-1    | Done in 0.74s.
app-1    | yarn run v1.22.19
app-1    | $ nodemon -L src/index.js
app-1    | [nodemon] 2.0.20
app-1    | [nodemon] to restart at any time, enter `rs`
app-1    | [nodemon] watching path(s): *.*
app-1    | [nodemon] watching extensions: js,mjs,json
app-1    | [nodemon] starting `node src/index.js`
app-1    | Waiting for mysql:3306........
app-1    | Connected!
app-1    | Connected to mysql db at host mysql
app-1    | Listening on port 3000
mysql-1  | 2024-01-23 17:22:08+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.36-1.el8 started.
mysql-1  | 2024-01-23 17:22:09+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
mysql-1  | 2024-01-23 17:22:09+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.36-1.el8 started.
mysql-1  | 2024-01-23 17:22:09+00:00 [Note] [Entrypoint]: Initializing database files
mysql-1  | 2024-01-23T17:22:09.699435Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
```

In case you want to take a look at specific service logs, like the app service, you just need to specify it at the end of the log command, like this: `docker compose logs -f app`

### When to use Dockerfile x Docker Compose

There's a big difference between using Dockerfile and using compose.yaml. Dockerfile is for making Docker images, while compose.yaml is for making and managing Docker services.

- **Dockerfile**: You can make a Docker image with a web application by using a file called a Dockerfile. The Dockerfile can make a Node.js image as the base of the image, copy the application's files to the image, and install the application's parts in the image.

- **compose.yaml**: You can make a Docker service by using a file called compose.yaml. The compose.yaml can show the image of the service that you made with the Dockerfile. It can decide which ports the service should use and how it should be set up.

In general, using Dockerfile is a good choice if you want to make a custom Docker image that fits your needs. Compose.yaml is a good choice if you want to make and manage many Docker services.
