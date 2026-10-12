EC servers in  "/home/emilcrona/servers/"

to run sh files do in the dir ./file_name.sh



## docker 
test docker env below
/home/Projekts/testdoc/ubunt.... 
docker compose up -d [-d for deattached]

	docker compose exec app bash [[to]] open bash in the compose if setup


```
FROM ubuntu:26.04

[[what]] the dir is going to be in the container
WORKDIR /home/tempusers


ENV DEBIAN_FRONTEND=noninteractive

  
[[i]] wanted to update the ubuntu container and install some packages 
RUN apt-get update && apt-get install -y nano curl vim && rm -rf /var/lib/apt/lists/*

  
[[this]] is the cmd to have the terminal in it and interact with it 
CMD ["/bin/bash"]
```





How my simpel compose file is setup works together with the dockerfile
```
services:

	app:

		build: .
[[to]] have the container interactive and to get keystrokes to be allowed
		stdin_open: true
[[to]] get the terminal formatting
		tty: true
		
[[to]] make the path mirrored to the dir in whcih the compose is alr in
		volumes:

			- .:/home/tempusers/Docker

		working_dir: /home/tempusers/Docker
```

