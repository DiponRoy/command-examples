

# Docker
<p align="left">
    <a href="https://www.docker.com/" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg" alt="docker" width="40" height="40" /> </a>
</p>

Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.
 -  **official**: https://docs.docker.com/engine/reference/run/

Other Ref

 - https://pspdfkit.com/blog/2018/how-to-manage-multiple-system-configurations-using-docker-compose/
 - https://dev.to/aduranil/10-docker-compose-and-docker-commands-that-are-useful-for-active-development-22f9

**Version**
```
docker -v			
docker --version
```

 
## Most Used Commands
**Install/Update/Uninstall package**
```
docker ps					# list running containers

docker-compose build		# build containers listed at docker-compose.yml
docker-compose up -d		# run containers listed at docker-compose.yml without logs
docker-compose down			# stop containers listed at docker-compose.yml

docker logs <container_id>	# show log of a container

docker exec -it <container_id> bash # open bash command cli of a container

docker restart <container_id>		# retart a container
docker stop <container_id>			# stop a container
docker rm <container_id>			# remove a container

docker kill <container_id>			# force stop a container
docker rm -f <container_id>			# force remove a container

docker system prune -a --volumes	# delete everything from docker https://stackoverflow.com/questions/44785585/how-to-delete-all-local-docker-images
docker system df                    # show storage and memory usages 

docker-compose build --no-cache					# build containers listed at docker-compose.yml without using cache
docker-compose build --no-cache service_name	# build a server listed at docker-compose.yml without using cache

docker-compose up -d --no-deps --build <service_name> # rebuild docker container from docker-compose.yml **https://stackoverflow.com/questions/36884991/how-to-rebuild-docker-container-in-docker-compose-yml
docker-compose build worker && docker-compose restart worker

docker-compose restart service_name			#restart service
docker-compose stop service_name			#stop service

docker volume ls								#show volume
docker volume rm <volume_name>					#remove volume
docker volume rm <volume_name1> <volume_name1>	#remove volumes

docker image ls		#list aviable image

docker container ls		#list running containers, same as 'docker ps'

docker-compose docker-compose.prod.yml up -d	# use docker-compose.prod.yml

```


## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
## need to add
