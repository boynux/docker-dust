Using Docker for dust computing
===============================

This is a simple PoC with Docker and nginx to show how *Dust* computing can be done using containers.
In this simple app upon any HTTP request a new container with NginX will be spawn off and the call will be proxied to that, for next one minute any subsequent calls from same clinet will be redirected to the same container and after one minutes the container will be removed by scheduled job.

The whole process is written using LUA and NginX with help of OpenResty project. So no other application expect Memcached (as container) needed.

How to run:
===========

1. Install Docker: `$ curl https://get.docker.com | sh`
2. Install Compose: `$ pip install docker-compose`
3. Start the app: `$ docker-compose up`
4. Open your browser and point it to `http://localhost:8080`

With the first request if you do `$ docker ps` you'll notice a new container has been started and after one minute it gets killed. Any subsequent call whithin one minute from the first request won't spawn new contaienr and will be delivered to the same container (from the same clinet IP).

Why it's intresting?
====================

* It's very secure since each client will be redirected to it's own container. 
* Finer resource management, since each container can be limited to subset of RAM and CPU so we can manage resources based on number of clinets
* Upgrades will be easy as each request spawns new container and it can get new updated container (not supported yet).
* Can be scaled easily by using Docke swarm and Overlay network we can spawn conatainer in distributed Docker hosts.

* Note: this is currently not secure since NginX proxy runs as a root permission, but it can be changed to some other user with some extra work. *

Feel free to try it out and edit the code. I'd love to hear you suggestions and ideas.
