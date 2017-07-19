# Nodeapp with Postgres and Docker


### this is an exercise from CodeSchool
* set up docker environment
* set up node inside docker
* set up postgres inside docker and connect to node


## the code below is for creating this app(details in src/server.js & Dockerfile & package.json):
-----------------------------

```
>>>docker build -t node-app .
>>>docker container run --name node-app --rm -p 8888:8888 node-app
[in new tab]
>>>docker ps
>>>docker stop node-app
>>>docker container run --name node-app --rm -p 8888:8888 -v ~/Desktop/testdocker/node/src:/usr/src/app/src node-app
>>>docker exec -it node-app /bin/bash
----[inside container]
>>>ls -la
>>>cd src
>>>ls -la
>>>cd views
>>>ls -la
>>>cd pages
[update server.js]
[update to github]

video:46'22''
[make postgres]
testdocker>>>mkdir postgres
>>>cd postgres 
[inside Docker file]
FROM  postgres:9.6
LABEL maintainer "xxxx@xxx.com"
EXPOSE 5432
>>>docker build -t node-db .
>>>docker container run --name node-db -p 9000:5432 --rm node-db:latest
>>>docker container exec -it node-db psql -U postgres
---[inside postgres]
>>>\l
>>>\dt
>>>create table votes(id integer primary key, number_of_votes integer, optional_name varchar(20));
>>>\dt
>>>insert into votes(id,number_of_votes,optional_name) values(1,0,'sandwiches');
>>>insert into votes(id,number_of_votes,optional_name) values(2,0,'tacos');
>>>SELECT * FROM votes;
>>>\q
[check docker ip address for server.js  client ,find 'Gateway']
>>>docker network inspect bridge
[update server.js]
[]
>>>docker container exec -it node-db psql -U postgres
---[inside postgres]
>>>update votes set number_of_votes=7 where id=2;
---[postgres folder create seed.sql]
---[inside postgres Dockerfile,add these]
>>>COPY seed.sql /docker-entrypoint-initdb.d/
>>>VOLUME /var/lib/postgresql/data
[create a new folder called 'data' under postgres and inside folder create .keep]
[check ]
>>>docker container ps
>>>docker build -t node-db .
>>>docker container run --name node-db -p 9000:5432 --rm -v ~/Desktop/testdocker/postgres/data:/var/lib/postgresql/dta node-db:latest
```


## this code is for first time to setup docker environment:

```
docker command line:
［docker default image：nginx ，using debian］
>>docker pull nginx
>>>docker container run nginx:latest

[in new tab to check running situation]
>>docker ps
[check all]
>>docker ps --all
or >>>docker ps －a

[default localhost -browser open]
localhost:80/index.html

[to stop container]
method1: "control-c"
method2: 
>>docker stop <CONTAINER ID>

[remove container]
>>docker rm <CONTAINER ID>
［remove it when it's stop］
or >>docker container run -p 9999:80 --name web-server --rm nginx:latest


[map localhost port to container's port-default is 80]
>>docker container run -p 9999:80 nginx:latest
localhost:9999/index.html

[name web server]
>>docker container run -p 9999:80 --name web-server nginx:latest

［go into container］
>>docker exec -it web-server /bin/bash


－－－［inside container］
>>>ls -la
>>>cd usr
>>>cd  share 
>>>cd nginx
>>>cd html
>>>ls -la
－－－－－［create your own html］
>>>touch page.html
>>>ls
>>>nano page.html
(you will find it uninstall,command not found)
－－－－－［add sth ］
>>>apt-get update
>>>apt-get install -y nano
>>>nano page.html
－－－［exit］
>>>exit

[map local file to container, inside local file,then:]
>>>docker container run -p 9999:80 --name web-server  --rm -v ~/Desktop/dockerTest/nginx/html:/usr/share/nginx/html nginx:latest

［automate the work，create Dockerfile in local file /nginx］
－－－［inside Dockerfile］
FROM nginx:latest
EXPOSE 80
RUN apt-get update && apt-get install -y nano
LABEL maintainer="susanlulu@outlook.com"
VOLUME /usr/share/nginx/html

[rename image tag]
>>>docker tag <image id> <repository name>:<tag name>
docker tag IMAGEID(镜像id) REPOSITORY:TAG（仓库：标签）

nginx>>docker build -t web-server . 
>>>docker image ls
>>>docker container run --name web-server --rm -p 9999:80 web-server:latest
24'58''
```
