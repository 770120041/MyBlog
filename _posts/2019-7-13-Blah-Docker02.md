---
layout: post
title:  "Docker-Example"
categories: Blah
---
# Create a Ubuntu with gcc
### Create a Docker File
```
FROM ubuntu
# ...
FROM ubuntu
RUN apt-get update
RUN apt-get -y install gcc
RUN apt-get -y install g++
RUN rm -rf /var/lib/apt/lists/*
```

### Run

In windows run `winpty docker run -it ubuntu bash`
 
In Linux, run `docker run -it ubuntu bash`

Then its done

### show and rm
Show containers `docker ps -a`

RM `docker container stop ID&docker container rm ID`

ls `docker container ls ID`

RM all `docker system prune `

### Get out and back
In windows: `CTRL-Z` to get out

And `winpty docker exec -it Container-ID bash` to go back 



# Docker is like a makefile
Dockerfile is like the makefile, and `.dockerignore` is like .gitignore

## Command details
[Official docs](https://docs.docker.com/engine/reference/builder)

When executing dockerfile, note that each instruction is run **independently**, and causes a new image to be created - so RUN cd /tmp will not have any effect on the next instructions.

Once a comment, empty line or builder instruction has been processed, Docker no longer looks for parser directives. Instead it treats anything formatted as a parser directive as a comment and does not attempt to validate if it might be a parser directive. Therefore, all parser directives must be at the very top of a Docke

