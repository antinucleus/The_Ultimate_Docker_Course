## SECTION 1,2 (Getting started and Introduction to docker)

- run this command for building images
    `docker build -t hello-docker .`
    . = docker file location
    -t = tag for our container
- `docker images` or `docker image ls`== shows images
- `docke run <NAME>` == start the container
- `docker run <NAME>` == check first if there is already this image docker run it other wise docker pull latest image for given name
- `docker run -it <NAME>` == run the image interactive mode

## SECTION 3 (The linux command line)

- `rm file*` == remove file which starts with given name
- `more <filename>` command shows the content of file like `cat` command but paginate the content but we only scrool to down with this command
- `less <filename> ` command shows the content of file like `cat` command but paginate the content 
- `head -n 5 <filename>` command shows the content of file like `cat` command. We can set line that shows first n line of the file with -n parameter 
- `tail -n 5 <filename>` command shows the content of file like `cat` command. We can set line that shows last n line of the file with -n parameter 
- `grep -i -r <word> <filename>` command search the word in given file (i = case insensetive) (r = recursively)
- `find -type d` command shows only the directories in the current directory
- `find -type f -name "f*"` command shows only the files in the current directory with given name (f* pattern means that all the files that starts with f letter)
- `find <directoryname>` we can pass the search directory
- `;` command chain the command each other (`mkdir test ; cd test ; echo done` == create a directory called test and change current directory to there and print the done message)
- `&&` this is the same as `;` operator but the difference is that; if there is an error any executing any command with `;` others still can be run but if we use 
`&&` if there is an error all the commands will be killed
- We can split the command using `;\` for example 
    ```
    mkdir test ; cd test ; echo done   

    // We can run this commands like this
    mkdir test;\    // this return line below
    > // here we can type another command ...
    ```
- `printenv` command print the environment variables
- `printenv <name>` command print the environment variables with given name (`printenv PATH`)
- `echo $name` command print the environment variables with given name (`echo $PATH`)
- `export <value> = <name>` command set a environment variable the current session
- if we want to set environment variable persisent we should save this into  .bashrc
- `sourve <name>` command reload the file
- `ps` show the process
- `sleep <time>` command delay with given time(seconds)
- `sleep 3 &` & operator put the process into backgound
- `kill <id>` command stop the process with given process id
- `useradd -m <name>` command add user into /etc/passwd location (useradd -m faruk)
- `usermod -s <name>` commad provide us to modify the record (usermod -s /bin/bash faruk)
- `docker exec -it -u <username>` we can open the terminal providing username which is already adjusted in /etc/passwd config file
- `adduser` command more interactive than `useradd`
- `groupadd <name>` command create a group
- every user have primary group and zero or more suplementary groups. When created a newuser OS create automatically new primary group for this user with same username
- `usermod -G <groupname> <username>` add user into given group
- `groups <username>`shows groups of this user
- `drwxrwxrwx`  = if first character is '-' this is a file if it is 'd' then it is a directory
    after first char ,first section (rws) is own user permission and second for group permission and the last one is for other user permissions
- `chmod u+x` or `chmod u-x` give or remove executable(x) permission to user
- `chmod g+x` or `chmod g-x` give or remove executable(x) permission to group
- `chmod g+x` or `chmod o-x` give or remove executable(x) permission to other

## SECTION 4 (Building images)

- Dockerfile instructions
- FROM
- WORKDIR
- COPY
- ADD
- RUN
- ENV
- EXPOSE
- USER
- CMD
- ENTRYPOINT

- For copying files into container we can use COPY and ADD
    - COPY :
        - For all files
            `COPY . /app`
        - For specific files
            `COPY package.json /app`
        - We can use []
            `COPY ["package.json","/app"]`
    - ADD :
        - Two more advantages over COPY
        - We can add file from url
            `ADD https://www.texts.com/a.txt /app`
        - We can add zip file and docke automatically uncompress the zip file
            `ADD test.zip /app`

- We can exclude files or directories so we can use .dockerignore
- EXPOSE provide us to set that container listen given port 
- We can run command after run the container
    `docker run react-app npm start`
- CMD runs commands at runtime and RUN runs commands at build time
- We can ovewrite the CMD command when we run the contaier if we don't do this we can use ENTRYPOINT instructions
- Also ENTRYPOINT can be overwritten but for this we have to pass --entrypoint flag
- `docker image prune` remove all the images
- `docker container prune` remove all the stopped container
- We can add a tag to images
    `docker build -t react-app:1 .`
- We can remove a tag from images
    `docker image remove react-app:1`
- We can save our images to file
    `docker image save -o react-app.tar react-app`
- We can load images
    `docker image load -i react-app.tar`

## SECTION 5 (Working with containers)

- `docker run -d <name,tag>` run the container in background
- We can give name to container `docker run -d --name blue-sky react-app`
- `docker logs <name>` command shows the output of container
- `docker logs -f <name>` command shows the output of container but follows its outputs
- `docker logs t <name>` command shows the output of container with time
- We can map port using -p flag
    `docker run -d -p 80:3000 --name c1 react-app`
- We execute a command in running container with `docker exec`
    `docker exec <container name> ls`
- `docker exec -it <container name> sh`
- `docker stop <container name>` stop the running container
- `docker start <container name>` start the stopped container
- `docker run <container name>` start a new container
- `docker rm <container name>` remove the container if the container does not run if it runs we can use -f (force) flag to remove it `docker rm -f <container name>`
- `docker volume create <name>` create a volume that includes our files inside here
- `docker run -d -p 4000:3000 -v app-data:/app/data react-app` creates a container and its volume named `app-data`
- We can copy files between host and container
    `docker cp <source> <destination>`
    - `docker cp 4c8:/app/test.txt .`  == copy file from the container to current directory
- `docker run -d -p 4000:3000 -v $(pwd):/app react-app` creates a container and its volume named full path of our current host so if we change any file it immediately change the container file
