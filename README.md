# Nodeapp with Postgres and Docker

this is an exercise from CodeSchool
-----------------------------------
		* set up docker environment
		* set up node inside docker
		* set up postgres inside docker and connect to node


###the code below is from video:

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
