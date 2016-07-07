# devops_challenge

Requirement
===========

- Upgrade the solution so it is possible to deploy a new version (based on image tactivos/devops-challenge:0.0.2) of the application while generating no downtime for users. Uptime should be verifiable with a health check by repeatedly calling the `/ping` endpoint on that same host.

My solution
===========
Currently I have not worked with docker so much, but I think a fast aproach is having  this containers architecture on the same host:
```
    user
      |
   haproxy
     /\
    /  \
   /    \
app1    app2
```

I use haproxy as a loadbalancing container between two app containers with the current image release.
When I want to upgrade apps container whithout downtime I run an script for removing app2 from LB pool, upgrade and then the same with app1.

How to run it
=============

Clone this repo with:
```
git clone git@github.com:mijhael3000/devops_challenge.git
cd devops_challenge
```

deploy VM and install docker 
============================
```
PLAYBOOK="./install_docker.yml" vagrant up
```

provision container instances
=============================
```
PLAYBOOK="./provision_docker_containers.yml" vagrant provision
```

Upgrade apps's container release
================================
```
PLAYBOOK="./update_image_containers.yml" vagrant provision
```

Before run the upgrade you can open other terminal and run this to check:

```
while true; do curl 192.168.33.11:80/ping -s -o /dev/null -w "%{http_code} \n"; sleep 1; done
```
Also, you can see the haproxy ui on 192.168.33.11:80 on your browser. user/pass: stats


##########################################
Bonus point 1
=============
- On the docker-compose.yml file I could use limit_mem and cpuset for each member of the service

Bonus point 2
=============

- To make my solution HA I sould use 3 vms and run swarm as a container cluster.

Bonus point 3
=============

- Service discovery tools like consul to centralize which service is on which host.


