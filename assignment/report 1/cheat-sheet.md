# Cheat sheets and checklists

- Student name: Stevens Bram
- Github repo: https://github.com/HoGentTIN/infra-2122-BramStevens1998

## Basic commands

| Task                 	| Command 							|
| :---                 	| :---   							|
| Query IP-adress(es)  	| `ip a`  							|
| dockerlab aanmaken   	| `vagrant up dockerlab` 			|
| dockerlab updaten    	| `vagrant provision dockerlab` 	|
| stat Docker engine	| `systemctl status docker`			|
| check TCP ports in use| `sudo ss -tlnp`					|
| list running Dockers 	| `docker ps (-a)`						|
| list Docker images	| `docker images`					|
| launch alpine interactively and shell | `docker run -i -t --name alpine alpine`|
| show alpine hostname | `docker exec -t alpine /bin/hostname` |
| show container ip | `docker exec -t alpine /sbin/ip a` |
| enter shell | docker exec -i -t alpine /bin/sh` |
| docker remove image | `docker rmi ID` |
| docker remove | `docker rm NAME` |


## Git workflow

Simple workflow for a personal project without other contributors:

| Task                                         | Command                   |
| :---                                         | :---                      |
| Current project status                       | `git status`              |
| Select files to be committed                 | `git add FILE...`         |
| Commit changes to local repository           | `git commit -m 'MESSAGE'` |
| Push local changes to remote repository      | `git push`                |
| Pull changes from remote repository to local | `git pull`                |

## Checklist network configuration

1. Is the IP-adress correct? `ip a`
2. Is the router/default gateway correct? `ip r -n`
3. Is a DNS-server available? `cat /etc/resolv.conf`

