# Lab Report: Lab 1 : Container virtulisation

## Login creds
	admin
	oud passwd bd
## Setup
	vagrant up dockerlab
	vagrant provision dockerlab

## Lab
### Deel 1.1 Set up the lab environment
- Web UI up and running

![Home, 4 october 2021](img/HOME.PNG?raw=true)

- Check status of the Docker engine

  ```console
  vagrant@dockerlab:~$ systemctl status docker
  ● docker.service - Docker Application Container Engine
       Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset>
       Active: active (running) since Mon 2021-11-29 08:52:51 UTC; 4min 41s ago
  TriggeredBy: ● docker.socket
         Docs: https://docs.docker.com
     Main PID: 959 (dockerd)
        Tasks: 26
       Memory: 133.7M
       CGroup: /system.slice/docker.service
               ├─ 959 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/cont>
               ├─1285 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-por>
               └─1297 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-por>
  ```

- Check network TCP server ports

  ```console
  vagrant@dockerlab:~$ sudo ss -tlnp
  State     Recv-Q    Send-Q       Local Address:Port        Peer Address:Port    Process
  LISTEN    0         4096               0.0.0.0:9000             0.0.0.0:*        users:(("docker-proxy",pid=1285,fd=4))
  LISTEN    0         4096            172.30.0.1:9323             0.0.0.0:*        users:(("dockerd",pid=959,fd=21))
  LISTEN    0         4096             127.0.0.1:44015            0.0.0.0:*        users:(("containerd",pid=700,fd=12))
  LISTEN    0         4096               0.0.0.0:111              0.0.0.0:*        users:(("rpcbind",pid=569,fd=4),("systemd",pid=1,fd=80))
  LISTEN    0         4096         127.0.0.53%lo:53               0.0.0.0:*        users:(("systemd-resolve",pid=571,fd=13))
  LISTEN    0         128                0.0.0.0:22               0.0.0.0:*        users:(("sshd",pid=716,fd=3))
  LISTEN    0         4096               0.0.0.0:8000             0.0.0.0:*        users:(("docker-proxy",pid=1297,fd=4))
  LISTEN    0         4096                  [::]:111                 [::]:*        users:(("rpcbind",pid=569,fd=6),("systemd",pid=1,fd=84))
  LISTEN    0         128                   [::]:22                  [::]:*        users:(("sshd",pid=716,fd=4))
  ```

- List running Docker containers

  ```console
  vagrant@dockerlab:~$ docker ps
  CONTAINER ID   IMAGE                    COMMAND        CREATED       STATUS         PORTS                                                      NAMES
  05f853e921e6   portainer/portainer-ce   "/portainer"   8 weeks ago   Up 6 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
  ```

- List Docker images

  ```console
  vagrant@dockerlab:~$ docker images
  REPOSITORY               TAG           IMAGE ID       CREATED        SIZE
  jenkins/jenkins          lts           e4ebf98bd6ca   7 weeks ago    441MB
  local                    static-site   512df4976a66   7 weeks ago    7.24MB
  mysql                    5.7           9f35042c6a98   2 months ago   448MB
  portainer/portainer-ce   latest        8377e6877145   2 months ago   251MB
  alpine                   latest        14119a10abf4   3 months ago   5.6MB
  tutum/hello-world        latest        31e17b0746e4   5 years ago    17.8MB
  ```

### Deel 1.2 Our first containers

- run hello-world

  ```console
  vagrant@dockerlab:~$ docker run hello-world
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  2db29710123e: Pull complete
  Digest: sha256:cc15c5b292d8525effc0f89cb299f1804f3a725c8d05e158653a563f15e4f685
  Status: Downloaded newer image for hello-world:latest
  
  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  
  To generate this message, Docker took the following steps:
   1. The Docker client contacted the Docker daemon.
   2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
      (amd64)
   3. The Docker daemon created a new container from that image which runs the
      executable that produces the output you are currently reading.
   4. The Docker daemon streamed that output to the Docker client, which sent it
      to your terminal.
  
  To try something more ambitious, you can run an Ubuntu container with:
   $ docker run -it ubuntu bash
  
  Share images, automate workflows, and more with a free Docker ID:
   https://hub.docker.com/
  
  For more examples and ideas, visit:
   https://docs.docker.com/get-started/
  ```

 - remove container

   ```console
   vagrant@dockerlab:~$ docker rmi hello-world
   Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container b3b181d5e100 is using its referenced image feb5d9fea6a5
   vagrant@dockerlab:~$ docker rm b3b181d5e100
   b3b181d5e100
   vagrant@dockerlab:~$ docker rmi hello-world
   Untagged: hello-world:latest
   Untagged: hello-world@sha256:cc15c5b292d8525effc0f89cb299f1804f3a725c8d05e158653a563f15e4f685
   Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
   Deleted: sha256:e07ee1baac5fae6a26f30cabfe54a36d3402f96afda318fe0a96cec4ca393359
   ```

 - show containers

   ```console
   vagrant@dockerlab:~$ docker container ls
   CONTAINER ID   IMAGE                    COMMAND        CREATED          STATUS          PORTS                                                      NAMES
   ba51fc757cdf   alpine                   "/bin/sh"      54 seconds ago   Up 53 seconds                                                              alpine
   05f853e921e6   portainer/portainer-ce   "/portainer"   8 weeks ago      Up 13 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
   ```

 - inspect alpine

   ```console
   vagrant@dockerlab:~$ docker inspect alpine
   [
       {
           "Id": "ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e",
           "Created": "2021-11-29T09:05:24.072730302Z",
           "Path": "/bin/sh",
           "Args": [],
           "State": {
               "Status": "running",
               "Running": true,
               "Paused": false,
               "Restarting": false,
               "OOMKilled": false,
               "Dead": false,
               "Pid": 3352,
               "ExitCode": 0,
               "Error": "",
               "StartedAt": "2021-11-29T09:05:24.870761567Z",
               "FinishedAt": "0001-01-01T00:00:00Z"
           },
           "Image": "sha256:c059bfaa849c4d8e4aecaeb3a10c2d9b3d85f5165c66ad3a4d937758128c4d18",
           "ResolvConfPath": "/var/lib/docker/containers/ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e/resolv.conf",
           "HostnamePath": "/var/lib/docker/containers/ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e/hostname",
           "HostsPath": "/var/lib/docker/containers/ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e/hosts",
           "LogPath": "/var/lib/docker/containers/ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e/ba51fc757cdfd47745da58f4e74243cc7f4c574c3849d8595bf2795bf952141e-json.log",
           "Name": "/alpine",
           "RestartCount": 0,
           "Driver": "overlay2",
           "Platform": "linux",
           "MountLabel": "",
           "ProcessLabel": "",
           "AppArmorProfile": "docker-default",
           "ExecIDs": null,
           "HostConfig": {
               "Binds": null,
               "ContainerIDFile": "",
               "LogConfig": {
                   "Type": "json-file",
                   "Config": {}
               },
               "NetworkMode": "default",
               "PortBindings": {},
               "RestartPolicy": {
                   "Name": "no",
                   "MaximumRetryCount": 0
               },
               "AutoRemove": false,
               "VolumeDriver": "",
               "VolumesFrom": null,
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
               "ConsoleSize": [
                   0,
                   0
               ],
               "Isolation": "",
               "CpuShares": 0,
               "Memory": 0,
               "NanoCpus": 0,
               "CgroupParent": "",
               "BlkioWeight": 0,
               "BlkioWeightDevice": [],
               "BlkioDeviceReadBps": null,
               "BlkioDeviceWriteBps": null,
               "BlkioDeviceReadIOps": null,
               "BlkioDeviceWriteIOps": null,
               "CpuPeriod": 0,
               "CpuQuota": 0,
               "CpuRealtimePeriod": 0,
               "CpuRealtimeRuntime": 0,
               "CpusetCpus": "",
               "CpusetMems": "",
               "Devices": [],
               "DeviceCgroupRules": null,
               "DeviceRequests": null,
               "KernelMemory": 0,
               "KernelMemoryTCP": 0,
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
                   "/sys/firmware"
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
                   "LowerDir": "/var/lib/docker/overlay2/b23c3ecdd6e93082322269158bbe47e2d802eb1e027660dde1a855578862d2fe-init/diff:/var/lib/docker/overlay2/f5d52a92b4f782105819e019ecf4a36b2758d2a10d8f606f11ca943d845956d0/diff",
                   "MergedDir": "/var/lib/docker/overlay2/b23c3ecdd6e93082322269158bbe47e2d802eb1e027660dde1a855578862d2fe/merged",
                   "UpperDir": "/var/lib/docker/overlay2/b23c3ecdd6e93082322269158bbe47e2d802eb1e027660dde1a855578862d2fe/diff",
                   "WorkDir": "/var/lib/docker/overlay2/b23c3ecdd6e93082322269158bbe47e2d802eb1e027660dde1a855578862d2fe/work"
               },
               "Name": "overlay2"
           },
           "Mounts": [],
           "Config": {
               "Hostname": "ba51fc757cdf",
               "Domainname": "",
               "User": "",
               "AttachStdin": true,
               "AttachStdout": true,
               "AttachStderr": true,
               "Tty": true,
               "OpenStdin": true,
               "StdinOnce": true,
               "Env": [
                   "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
               ],
               "Cmd": [
                   "/bin/sh"
               ],
               "Image": "alpine",
               "Volumes": null,
               "WorkingDir": "",
               "Entrypoint": null,
               "OnBuild": null,
               "Labels": {}
           },
           "NetworkSettings": {
               "Bridge": "",
               "SandboxID": "b58d5c0035c4197d52362fc104be203cb8d811d60b1d826239fb82c1d8675475",
               "HairpinMode": false,
               "LinkLocalIPv6Address": "",
               "LinkLocalIPv6PrefixLen": 0,
               "Ports": {},
               "SandboxKey": "/var/run/docker/netns/b58d5c0035c4",
               "SecondaryIPAddresses": null,
               "SecondaryIPv6Addresses": null,
               "EndpointID": "247cd7421aedd6e5b00b98ce8f50931c1005e0a55a248d3cc983463e4df61a83",
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
                       "NetworkID": "2a5ee03d2ad6c457a7aa294c5e78a69469c47ede5f5c30f9b8dd8857c16d7dfd",
                       "EndpointID": "247cd7421aedd6e5b00b98ce8f50931c1005e0a55a248d3cc983463e4df61a83",
                       "Gateway": "172.17.0.1",
                       "IPAddress": "172.17.0.2",
                       "IPPrefixLen": 16,
                       "IPv6Gateway": "",
                       "GlobalIPv6Address": "",
                       "GlobalIPv6PrefixLen": 0,
                       "MacAddress": "02:42:ac:11:00:02",
                       "DriverOpts": null
                   }
               }
           }
       }
   ]
   ```

 - top alpine

   ```console
   vagrant@dockerlab:~$ docker top alpine
   UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
   root                3352                3326                0                   09:05               pts/0               00:00:00            /bin/sh
   ```

 - alpine detached

   ```console
   vagrant@dockerlab:~$ docker run -i -t -d --name alpine alpine
   Unable to find image 'alpine:latest' locally
   latest: Pulling from library/alpine
   59bf1c3509f3: Pull complete
   Digest: sha256:21a3deaa0d32a8057914f36584b5288d2e5ecc984380bc0118285c70fa8c9300
   Status: Downloaded newer image for alpine:latest
   a2b6d5b252ffe76782b46685604e79af604780fcb699810424467d0d39363688
   ```

 - hostname container

   ```console
   vagrant@dockerlab:~$ docker exec -t alpine /bin/hostname
   a2b6d5b252ff
   ```

 - ip address container

   ```console
   vagrant@dockerlab:~$ docker exec -t alpine /sbin/ip a
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
   12: eth0@if13: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
       link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
       inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
          valid_lft forever preferred_lft forever
   ```

 - exit shell, still up?

   ```console
   vagrant@dockerlab:~$ docker ps
   CONTAINER ID   IMAGE                    COMMAND        CREATED         STATUS          PORTS                                                      NAMES
   a2b6d5b252ff   alpine                   "/bin/sh"      4 minutes ago   Up 4 minutes                                                               alpine
   05f853e921e6   portainer/portainer-ce   "/portainer"   8 weeks ago     Up 23 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
   vagrant@dockerlab:~$ docker ps -a
   CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                     PORTS                                                      NAMES
   a2b6d5b252ff   alpine                   "/bin/sh"                4 minutes ago   Up 4 minutes                                                                          alpine
   7dc209ca838e   jenkins/jenkins:lts      "/sbin/tini -- /usr/…"   6 weeks ago     Exited (130) 6 weeks ago                                                              jenkins_server
   5debdc784953   local:static-site        "nginx -g 'daemon of…"   7 weeks ago     Exited (0) 7 weeks ago                                                                nginx
   ccfeea6b9d8e   mysql:5.7                "docker-entrypoint.s…"   7 weeks ago     Exited (0) 7 weeks ago                                                                db
   49b0d90eb2d8   tutum/hello-world        "/bin/sh -c 'php-fpm…"   7 weeks ago     Exited (0) 7 weeks ago                                                                helloapp
   05f853e921e6   portainer/portainer-ce   "/portainer"             8 weeks ago     Up 23 minutes              0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
   ```

 - tutum hello world

   ```console
   vagrant@dockerlab:~$ docker pull tutum/hello-world
   Using default tag: latest
   latest: Pulling from tutum/hello-world
   Image docker.io/tutum/hello-world:latest uses outdated schema1 manifest format. Please upgrade to a schema2 image for better future compatibility. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/
   658bc4dc7069: Already exists
   a3ed95caeb02: Already exists
   af3cc4b92fa1: Already exists
   d0034177ece9: Already exists
   983d35417974: Already exists
   Digest: sha256:0d57def8055178aafb4c7669cbc25ec17f0acdab97cc587f30150802da8f8d85
   Status: Image is up to date for tutum/hello-world:latest
   docker.io/tutum/hello-world:latest
   vagrant@dockerlab:~$ docker run -d -p 80 --name helloapp tutum/hello-world
   ceb72dda54f66bec26d6713683debaf1212a8827cbcd4bb8f6519eaaa165f5ee
   ```

 - check container

   ```console
   vagrant@dockerlab:~$ curl http://192.168.56.20:9000/
   <!DOCTYPE html
   ><html lang="en" ng-app="portainer" ng-strict-di>
     <head>
       <meta charset="utf-8" />
       <title>Portainer</title>
       <meta name="description" content="" />
       <meta name="author" content="Portainer.io" />
   
       <!-- HTML5 shim, for IE6-8 support of HTML5 elements -->
       <!--[if lt IE 9]>
         <script src="//html5shim.googlecode.com/svn/trunk/html5.js"></script>
       <![endif]-->
   
       <!-- Fav and touch icons -->
       <link rel="apple-touch-icon" sizes="180x180" href="dc4d092847be46242d8c013d1bc7c494.png" />
       <link rel="icon" type="image/png" sizes="32x32" href="5ba13dcb526292ae707310a54e103cd1.png" />
       <link rel="icon" type="image/png" sizes="16x16" href="f9508a64a1beb81be174e194573f7450.png" />
       <link rel="mask-icon" href="07745d55b001c85826eedd479285cdbb.svg" color="#5bbad5" />
       <link rel="shortcut icon" href="data:image/vnd.microsoft.icon;base64,AAABAAMAMDAAAAEAIACoJQAANgAAACAgAAABACAAqBAAAN4lAAAQEAAAAQAgAGgEAACGNgAAKAAAADAAAABgAAAAAQAgAAAAAAAAJAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAT/2jgEAAAAAP/aOAT/2jh8/9o4x//aONj/2jjH/9o4fP/aOBoAAAAAAAAAAAAAAAD/2jgE/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAAAAAAAAAAAA/9o4BP/aOMf/2jj//9o4///aOP//2jj//9o4///aONj/2jgy/9o4fP/aONj/2jjw/9o42P/aOK3/2jgaAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4Gv/aOFr/2jhG/9o4rf/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jjw/9o4///aOP//2jj//9o4///aOP//2jjw/9o4MgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jh8/9o48P/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o42P/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOJL/2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOFoAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4Mv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOJIAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4fP/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOHwAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4kv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOEYAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4fP/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4xwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Mv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4xwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOJL/2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o42AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jiS/9o48P/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4rQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Mv/aOFr/2jh8/9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4WgAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAD/2jha/9o48P/aONj/2jhG/9o4rf/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jitAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOEb/2jj//9o4///aOP//2jjH/9o4Gv/aOPD/2jj//9o4///aOP//2jj//9o4///aONj/2jiS/9o48P/aOP//2jj//9o48P/aOHz/2jgaAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aONj/2jj//9o4///aOP//2jj//9o4kv/aOBr/2jiS/9o48P/aOPD/2jjw/9o4kv/aOEb/2jgE/9o4Gv/aOEb/2jha/9o4Gv/aOHz/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Rv/aOP//2jj//9o4///aOP//2jj//9o4///aOMf/2jha/9o4Rv/aOEb/2jhG/9o4fP/aOPD/2jhGAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4fP/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOPD/2jj//9o4///aOP//2jh8AAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4kv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o48P/aOP//2jh8AAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4kv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jh8AAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Wv/aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jhGAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4Gv/aONj/2jj//9o48P/aOPD/2jj//9o48P/aOPD/2jjw/9o4///aOPD/2jjw/9o4///aONj/2jgEAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOFr/2jiS/9o4fP/aOHz/2jha/9o4fP/aOHz/2jh8/9o4Wv/aOHz/2jh8/9o4kv/aOEYAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jit/9o4rf/aOJL/2jiS/9o4rf/aOK3/2jit/9o4kv/aOK3/2jit/9o4rf/aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jh8/9o4fP/aOHz/2jiS/9o4fP/aOHz/2jh8/9o4kv/aOHz/2jh8/9o4fP/aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jit/9o4kv/aOJL/2jiS/9o4kv/aOJL/2jiS/9o4kv/aOK3/2jiS/9o4rf/aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOFr/2jit/9o4rf/aOJL/2jha/9o4rf/aOK3/2jit/9o4Wv/aOHz/2jh8/9o4fP/aODIAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jit/9o4rf/aOJL/2jh8/9o4rf/aOK3/2jit/9o4RgAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jiS/9o4fP/aOHz/2jiS/9o4fP/aOHz/2jiS/9o4Rv/aODL/2jgy/9o4Mv/aOBoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOHz/2jiS/9o4kv/aOJL/2jiS/9o4kv/aOJL/2jiS/9o4kv/aOMf/2jit/9o4x//aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOFr/2jiS/9o4kv/aOHz/2jha/9o4kv/aOJL/2jiS/9o4fP/aOJL/2jh8/9o4fP/aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Rv/aOK3/2jiS/9o4kv/aOFoAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgE/9o4BP/aOAQAAAAA/9o4BP/aOAQAAAAA/9o4Wv/aOMf/2jh8/9o4x//aOHwAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Wv/aOHwAAAAA/9o4Wv/aOHwAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAP/aOAT/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Wv/aOHwAAAAA/9o4Wv/aOHwAAAAAAAAAAP/aODL/2jj//9o4Mv/aOK3/2jjHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jga/9o4Mv/aODL/2jgy/9o4Mv/aODL/2jgy/9o4Mv/aODL/2jga/9o4fP/aOJL/2jga/9o4fP/aOJL/2jga/9o4Mv/aOFr/2jj//9o4Wv/aOMf/2jjY/9o4Gv/aODL/2jgy/9o4Mv/aODL/2jgy/9o4Mv/aODL/2jgy/9o4Mv/aODL/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAD/2jh8/9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jj//9o4///aOP//2jhGAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jga/9o4Rv/aOEb/2jhG/9o4Rv/aOEb/2jh8/9o48P/aOP//2jjH/9o4Rv/aODL/2jhG/9o4Rv/aOEb/2jhG/9o4Rv/aOFr/2jj//9o4fP/aODL/2jhG/9o4Rv/aOEb/2jhG/9o4Mv/aOEb/2jjH/9o4///aOPD/2jh8/9o4Rv/aOEb/2jgaAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4Gv/aOJL/2jj//9o42P/aOEYAAAAAAAAAAAAAAAAAAAAAAAAAAP/aODL/2jj//9o4RgAAAAD/2jgEAAAAAAAAAAAAAAAA/9o4Rv/aONj/2jj//9o4kv/aOBoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aOAT/2jgE/9o4BP/aOAT/2jgEAAAAAAAAAAD/2jgy/9o4x//aOP//2jjH/9o4MgAAAAD/2jgEAAAAAP/aODL/2jj//9o4RgAAAAD/2jgEAAAAAP/aODL/2jjH/9o4///aOMf/2jgyAAAAAAAAAAD/2jgE/9o4BP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAAAAAAAP/aOFr/2jjw/9o4///aOJL/2jgaAAAAAP/aODL/2jj//9o4RgAAAAD/2jga/9o4kv/aOP//2jjw/9o4WgAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgE/9o4BAAAAAD/2jgE/9o4fP/aOP//2jjw/9o4Wv/aODL/2jj//9o4Rv/aOFr/2jjw/9o4///aOHz/2jgEAAAAAP/aOAT/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAAAAAAAP/aOBr/2jit/9o4///aOPD/2jj//9o48P/aOP//2jit/9o4GgAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAAAAAAA/9o4Mv/aONj/2jj//9o42P/aOEYAAAAAAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aODL/2jj//9o4RgAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aODL/2jjw/9o4RgAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP///////wAA//+P////AAD//gMP//8AAP/8AAf//wAA/+AAA///AAD/gAAD//8AAP+AAAH//wAA/4AAA///AAD/AAAD//8AAP+AAAP//wAA/4AAA///AAD/gAAD//8AAP/AAAP//wAA//wAB///AAD/5AAH//8AAP/CAB///wAA/4EH9///AAD/gPun//8AAP+AA6f//wAA/wADp///AAD/AAOn//8AAP+AA6f//wAA/4ADp///AAD/3/en//8AAP/AB6f//wAA//u/p///AAD/wAen//8AAP/Ef6f//wAA/8R/p///AAD/23+n//8AAP/AB6f//wAA/8xfp///AAD//8en//8AAP//16f//wAA////p///AAD///+n//8AAP//26f//wAA/wAAAAD/AAD//H+/x/8AAP/+P7+P/wAA//+Pvj//AAD//+O4//8AAP//+bP//wAA///8B///AAD///8f//8AAP///7///wAA////v///AAD///////8AACgAAAAgAAAAQAAAAAEAIAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aOAT/2jgEAAAAAP/aOCP/2jhA/9o4IwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAT/2jic/9o4/v/aOP7/2jj+/9o4Y//aOCP/2jic/9o4nP/aOGMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOED/2jiO/9o4tP/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOI4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jiO/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOCMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4QP/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4YwAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAD/2jhj/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jhAAAAAAP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOED/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o40gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aONL/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jjS/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4I//aOI7/2jjS/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOLQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgE/9o4jv/aOID/2jiO/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4QAAAAAD/2jgEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOJz/2jj+/9o4/v/aOGP/2ji0/9o4/v/aOP7/2jj+/9o4jv/aOI7/2jjS/9o4tP/aOID/2jgjAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgj/9o4/v/aOP7/2jj+/9o46//aOID/2jiA/9o4jv/aOI7/2jiAAAAAAP/aOGP/2jhj/9o4tP/aOEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAP/aOGP/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOOv/2jjr/9o4/v/aOOsAAAAA/9o4gP/aOI7/2ji0/9o4QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4Y//aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o46wAAAAD/2jiA/9o4jv/aOLT/2jhAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jhA/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jj+/9o4/v/aOP7/2jjSAAAAAP/aOID/2jiO/9o4tP/aOEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jiO/9o4tP/aOJz/2jic/9o4tP/aOJz/2jic/9o4tP/aOEAAAAAA/9o4gP/aOI7/2ji0/9o4QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOID/2jiO/9o4gP/aOI7/2jiO/9o4gP/aOI7/2jiO/9o4QAAAAAD/2jiA/9o4jv/aOLT/2jhAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4gP/aOI7/2jiO/9o4jv/aOI7/2jiO/9o4nP/aOJz/2jhAAAAAAP/aOID/2jiO/9o4tP/aOEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jiA/9o4tP/aOID/2jiO/9o4tP/aOGP/2jhA/9o4QP/aOCMAAAAA/9o4gP/aOI7/2ji0/9o4QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOID/2jiO/9o4gP/aOI7/2jiO/9o4Y//aOAT/2jgj/9o4BAAAAAD/2jiA/9o4jv/aOLT/2jhAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4gP/aOLT/2jiO/9o4nP/aOJz/2jiO/9o4nP/aOLT/2jhAAAAAAP/aOID/2jiO/9o4tP/aOEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgj/9o4I//aOCP/2jgj/9o4I//aOED/2jiO/9o4jv/aOEAAAAAA/9o4gP/aOI7/2ji0/9o4QAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAP/aOAQAAAAA/9o4QP/aOI7/2jiA/9o4YwAAAAD/2jiO/9o4jv/aOLT/2jhAAAAAAP/aOAT/2jgE/9o4BP/aOAT/2jgE/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jhA/9o4QP/aOCP/2jhjAAAAAP/aOID/2jiO/9o4tP/aOEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aOID/2jiO/9o4gP/aOID/2jiA/9o4Y//aOJz/2ji0/9o4nP/aOLT/2jiA/9o40v/aONL/2jjS/9o4nP/aOID/2jiO/9o4gP/aOGP/2jiA/9o4jv/aOGMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgE/9o4jv/aOJz/2jiO/9o4nP/aOOv/2jj+/9o4tP/aOID/2jiO/9o4jv/aOI7/2jjS/9o40v/aOID/2jic/9o4jv/aOID/2ji0/9o4/v/aOOv/2jic/9o4gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aOID/2jjS/9o4YwAAAAD/2jgEAAAAAP/aOID/2jicAAAAAP/aOAQAAAAA/9o4Y//aONL/2jiA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAT/2jgE/9o4BP/aOAQAAAAAAAAAAP/aOCP/2ji0/9o46//aOGMAAAAA/9o4gP/aOJwAAAAA/9o4Y//aOOv/2ji0/9o4IwAAAAAAAAAA/9o4BP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgEAAAAAAAAAAD/2jhA/9o40v/aOLT/2jic/9o4tP/aOLT/2jjr/9o4QAAAAAAAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BAAAAAAAAAAA/9o4Y//aOOv/2jjr/9o4gP/aOAQAAAAA/9o4BAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4BP/aOAQAAAAA/9o4gP/aOJwAAAAA/9o4BP/aOAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgj/9o4IwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA///////DP///AA///AAP//wAD//8AA///AAP//wAD//+AA///gAf//xAH//8Ad///AEf//wBH//8AR///AMf//wDH//8Ax///B8f//wfH//8Ax////Mf///zH////x//+CABP/gAAB//zzz///Mz///8D////x////8///////8oAAAAEAAAACAAAAABACAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jh4/9o4rv/aODX/2jhW/9o4HwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOGj/2jjL/9o4/f/aOP3/2jj9/9o4/f/aOOL/2jgMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aODX/2jj9/9o4/f/aOP3/2jj9/9o4/f/aOP3/2jj9/9o4NQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgf/9o44v/aOP3/2jj9/9o4/f/aOP3/2jj9/9o44v/aOAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOFb/2jjL/9o4/f/aOP3/2jj9/9o4/f/aOK4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOAz/2jji/9o4y//aOK7/2jjL/9o4eP/aOJL/2jhoAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jg1/9o4/f/aOP3/2jj9/9o4/f/aOHj/2jh4/9o4eAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4DP/aOMv/2jjL/9o4y//aOOL/2jg1/9o4hv/aOHgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jh4/9o4hv/aOIb/2jiS/9o4H//aOIb/2jh4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/9o4kv/aOK7/2jiG/9o4Nf/aOAz/2jiG/9o4eAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOFb/2jhW/9o4aP/aOJL/2jgf/9o4hv/aOHgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/aOB//2jho/9o4H//aOIb/2jh4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jg1/9o4eP/aOJL/2jiu/9o4eP/aOHj/2jjL/9o4kv/aOGj/2jiu/9o4kv/aODUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgM/9o4aP/aOIb/2jgf/9o4aP/aOB//2jiG/9o4aP/aOAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jgf/9o4eP/aOMv/2jh4/9o4HwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD/2jhWAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP3/AADwPwAA4D8AAOA/AADwPwAA4X8AAOH/AADhfwAA8X8AAON/AAD9fwAA/38AAPMnAAD93wAA/38AAP//AAA=" />
       <meta name="msapplication-config" content="4806ce9049e1e082dd3da4063ceb0eea.xml" />
       <meta name="theme-color" content="#ffffff" />
     <link href="vendor.1.css" rel="stylesheet"><link href="main.363ebc3d2b9d33aca0b0.css" rel="stylesheet"></head>
   
     <body ng-controller="MainController">
       <div
         id="page-wrapper"
         ng-class="{
           open: toggle && ['portainer.auth', 'portainer.init.admin', 'portainer.init.endpoint'].indexOf($state.current.name) === -1,
           nopadding: ['portainer.auth', 'portainer.init.admin', 'portainer.init.endpoint', 'portainer.logout'].indexOf($state.current.name) > -1 || applicationState.loading
         }"
         ng-cloak
       >
         <div id="sideview" ui-view="sidebar" ng-if="!applicationState.loading"></div>
   
         <div id="content-wrapper">
           <div class="page-content">
             <div class="page-wrapper" ng-if="applicationState.loading">
               <!-- loading box -->
               <div class="container simple-box">
                 <div class="col-md-6 col-md-offset-3 col-sm-6 col-sm-offset-3">
                   <!-- loading box logo -->
                   <div class="row">
                     <img ng-if="logo" ng-src="{{ logo }}" class="simple-box-logo" />
                     <img ng-if="!logo" src="5da83cfb4883a59354abeff852cb7394.png" class="simple-box-logo" alt="Portainer" />
                   </div>
                   <!-- !loading box logo -->
                   <!-- panel -->
                   <div class="row" style="text-align: center;">
                     Loading Portainer...
                     <i class="fa fa-cog fa-spin" style="margin-left: 5px;"></i>
                   </div>
                   <!-- !panel -->
                 </div>
               </div>
               <!-- !loading box -->
             </div>
   
             <!-- Main Content -->
             <div id="view" ui-view="content" ng-if="!applicationState.loading"></div> </div
           ><!-- End Page Content --> </div
         ><!-- End Content Wrapper --> </div
       ><!-- End Page Wrapper -->
     <script type="text/javascript" src="vendor.363ebc3d2b9d33aca0b0.js"></script><script type="text/javascript" src="main.363ebc3d2b9d33aca0b0.js"></script></body></html
   >
   ```

 - Hello world tutum forward

``` console
    $ curl: http://localhost:49153/
<html>
<head>
        <title>Hello world!</title>
        <link href='http://fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
        <style>
        body {
                background-color: white;
                text-align: center;
                padding: 50px;
                font-family: "Open Sans","Helvetica Neue",Helvetica,Arial,sans-serif;
        }

        #logo {
                margin-bottom: 40px;
        }
        </style>
</head>
<body>
        <img id="logo" src="logo.png" />
        <h1>Hello world!</h1>
        <h3>My hostname is 49b0d90eb2d8</h3>    </body>
</html>
```

![Hello-world, 4 october 2021](img/Hello-world.PNG?raw=true)


### Deel 1.3 Persistent data

- create and inspect Mysql-data

```console
vagrant@dockerlab:~$ docker volume create mysql-data
mysql-data
vagrant@dockerlab:~$ docker volume inspect mysql-data
[
    {
        "CreatedAt": "2021-10-04T17:50:42Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/mysql-data/_data",
        "Name": "mysql-data",
        "Options": {},
        "Scope": "local"
    }
]

```

- adding volume to a container and check logs

```console
2021-10-04 18:02:39+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.35-1debian10 started.
2021-10-04 18:02:40+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2021-10-04 18:02:40+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.35-1debian10 started.
2021-10-04 18:02:40+00:00 [Note] [Entrypoint]: Initializing database files
2021-10-04T18:02:40.158910Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-10-04T18:02:40.376824Z 0 [Warning] InnoDB: New log files created, LSN=45790
2021-10-04T18:02:40.503459Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2021-10-04T18:02:40.632105Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 4409133b-253d-11ec-bcae-0242ac110003.
2021-10-04T18:02:40.636520Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2021-10-04T18:02:41.220486Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:41.220597Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:41.221266Z 0 [Warning] CA certificate ca.pem is self signed.
2021-10-04T18:02:41.474092Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2021-10-04 18:02:44+00:00 [Note] [Entrypoint]: Database files initialized
2021-10-04 18:02:44+00:00 [Note] [Entrypoint]: Starting temporary server
2021-10-04 18:02:44+00:00 [Note] [Entrypoint]: Waiting for server startup
2021-10-04T18:02:45.107611Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-10-04T18:02:45.109024Z 0 [Note] mysqld (mysqld 5.7.35) starting as process 77 ...
2021-10-04T18:02:45.112839Z 0 [Note] InnoDB: PUNCH HOLE support available
2021-10-04T18:02:45.112855Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2021-10-04T18:02:45.112879Z 0 [Note] InnoDB: Uses event mutexes
2021-10-04T18:02:45.112883Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
2021-10-04T18:02:45.112887Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2021-10-04T18:02:45.112891Z 0 [Note] InnoDB: Using Linux native AIO
2021-10-04T18:02:45.113212Z 0 [Note] InnoDB: Number of pools: 1
2021-10-04T18:02:45.113408Z 0 [Note] InnoDB: Using CPU crc32 instructions
2021-10-04T18:02:45.114801Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
2021-10-04T18:02:45.163153Z 0 [Note] InnoDB: Completed initialization of buffer pool
2021-10-04T18:02:45.177884Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
2021-10-04T18:02:45.196683Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
2021-10-04T18:02:45.212617Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2021-10-04T18:02:45.212704Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2021-10-04T18:02:45.272214Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2021-10-04T18:02:45.272868Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
2021-10-04T18:02:45.272882Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
2021-10-04T18:02:45.273182Z 0 [Note] InnoDB: Waiting for purge to start
2021-10-04T18:02:45.341339Z 0 [Note] InnoDB: 5.7.35 started; log sequence number 2748441
2021-10-04T18:02:45.341664Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2021-10-04T18:02:45.341695Z 0 [Note] Plugin 'FEDERATED' is disabled.
2021-10-04T18:02:45.350626Z 0 [Note] InnoDB: Buffer pool(s) load completed at 211004 18:02:45
2021-10-04T18:02:45.375521Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
2021-10-04T18:02:45.375537Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
2021-10-04T18:02:45.375540Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:45.375562Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:45.376365Z 0 [Warning] CA certificate ca.pem is self signed.
2021-10-04T18:02:45.376416Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
2021-10-04T18:02:45.380983Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2021-10-04T18:02:45.395303Z 0 [Note] Event Scheduler: Loaded 0 events
2021-10-04T18:02:45.395513Z 0 [Note] mysqld: ready for connections.
Version: '5.7.35'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server (GPL)
2021-10-04 18:02:45+00:00 [Note] [Entrypoint]: Temporary server started.
Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
2021-10-04 18:02:48+00:00 [Note] [Entrypoint]: Creating database appdb
2021-10-04 18:02:48+00:00 [Note] [Entrypoint]: Creating user appusr
2021-10-04 18:02:48+00:00 [Note] [Entrypoint]: Giving user appusr access to schema appdb

2021-10-04 18:02:48+00:00 [Note] [Entrypoint]: Stopping temporary server
2021-10-04T18:02:48.305952Z 0 [Note] Giving 0 client threads a chance to die gracefully
2021-10-04T18:02:48.305975Z 0 [Note] Shutting down slave threads
2021-10-04T18:02:48.305984Z 0 [Note] Forcefully disconnecting 0 remaining clients
2021-10-04T18:02:48.305990Z 0 [Note] Event Scheduler: Purging the queue. 0 events
2021-10-04T18:02:48.306024Z 0 [Note] Binlog end
2021-10-04T18:02:48.306690Z 0 [Note] Shutting down plugin 'ngram'
2021-10-04T18:02:48.306701Z 0 [Note] Shutting down plugin 'partition'
2021-10-04T18:02:48.306704Z 0 [Note] Shutting down plugin 'BLACKHOLE'
2021-10-04T18:02:48.306726Z 0 [Note] Shutting down plugin 'ARCHIVE'
2021-10-04T18:02:48.306729Z 0 [Note] Shutting down plugin 'PERFORMANCE_SCHEMA'
2021-10-04T18:02:48.306749Z 0 [Note] Shutting down plugin 'MRG_MYISAM'
2021-10-04T18:02:48.306766Z 0 [Note] Shutting down plugin 'MyISAM'
2021-10-04T18:02:48.306772Z 0 [Note] Shutting down plugin 'INNODB_SYS_VIRTUAL'
2021-10-04T18:02:48.306775Z 0 [Note] Shutting down plugin 'INNODB_SYS_DATAFILES'
2021-10-04T18:02:48.306777Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESPACES'
2021-10-04T18:02:48.306780Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN_COLS'
2021-10-04T18:02:48.306782Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN'
2021-10-04T18:02:48.306784Z 0 [Note] Shutting down plugin 'INNODB_SYS_FIELDS'
2021-10-04T18:02:48.306787Z 0 [Note] Shutting down plugin 'INNODB_SYS_COLUMNS'
2021-10-04T18:02:48.306789Z 0 [Note] Shutting down plugin 'INNODB_SYS_INDEXES'
2021-10-04T18:02:48.306791Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESTATS'
2021-10-04T18:02:48.306793Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLES'
2021-10-04T18:02:48.306796Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_TABLE'
2021-10-04T18:02:48.306798Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_CACHE'
2021-10-04T18:02:48.306800Z 0 [Note] Shutting down plugin 'INNODB_FT_CONFIG'
2021-10-04T18:02:48.306802Z 0 [Note] Shutting down plugin 'INNODB_FT_BEING_DELETED'
2021-10-04T18:02:48.306804Z 0 [Note] Shutting down plugin 'INNODB_FT_DELETED'
2021-10-04T18:02:48.306807Z 0 [Note] Shutting down plugin 'INNODB_FT_DEFAULT_STOPWORD'
2021-10-04T18:02:48.306809Z 0 [Note] Shutting down plugin 'INNODB_METRICS'
2021-10-04T18:02:48.306811Z 0 [Note] Shutting down plugin 'INNODB_TEMP_TABLE_INFO'
2021-10-04T18:02:48.306813Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_POOL_STATS'
2021-10-04T18:02:48.306816Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE_LRU'
2021-10-04T18:02:48.306818Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE'
2021-10-04T18:02:48.306820Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX_RESET'
2021-10-04T18:02:48.306823Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX'
2021-10-04T18:02:48.306825Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM_RESET'
2021-10-04T18:02:48.306827Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM'
2021-10-04T18:02:48.306829Z 0 [Note] Shutting down plugin 'INNODB_CMP_RESET'
2021-10-04T18:02:48.306832Z 0 [Note] Shutting down plugin 'INNODB_CMP'
2021-10-04T18:02:48.306834Z 0 [Note] Shutting down plugin 'INNODB_LOCK_WAITS'
2021-10-04T18:02:48.306836Z 0 [Note] Shutting down plugin 'INNODB_LOCKS'
2021-10-04T18:02:48.306838Z 0 [Note] Shutting down plugin 'INNODB_TRX'
2021-10-04T18:02:48.306840Z 0 [Note] Shutting down plugin 'InnoDB'
2021-10-04T18:02:48.306899Z 0 [Note] InnoDB: FTS optimize thread exiting.
2021-10-04T18:02:48.307194Z 0 [Note] InnoDB: Starting shutdown...
2021-10-04T18:02:48.408297Z 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool
2021-10-04T18:02:48.408635Z 0 [Note] InnoDB: Buffer pool(s) dump completed at 211004 18:02:48
2021-10-04T18:02:50.314790Z 0 [Note] InnoDB: Shutdown completed; log sequence number 12665889
2021-10-04T18:02:50.316299Z 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
2021-10-04T18:02:50.316316Z 0 [Note] Shutting down plugin 'MEMORY'
2021-10-04T18:02:50.316339Z 0 [Note] Shutting down plugin 'CSV'
2021-10-04T18:02:50.316343Z 0 [Note] Shutting down plugin 'sha256_password'
2021-10-04T18:02:50.316346Z 0 [Note] Shutting down plugin 'mysql_native_password'
2021-10-04T18:02:50.316596Z 0 [Note] Shutting down plugin 'binlog'
2021-10-04T18:02:50.317702Z 0 [Note] mysqld: Shutdown complete

2021-10-04 18:02:51+00:00 [Note] [Entrypoint]: Temporary server stopped

2021-10-04 18:02:51+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

2021-10-04T18:02:51.569300Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-10-04T18:02:51.570548Z 0 [Note] mysqld (mysqld 5.7.35) starting as process 1 ...
2021-10-04T18:02:51.574552Z 0 [Note] InnoDB: PUNCH HOLE support available
2021-10-04T18:02:51.574569Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2021-10-04T18:02:51.574592Z 0 [Note] InnoDB: Uses event mutexes
2021-10-04T18:02:51.574597Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
2021-10-04T18:02:51.574601Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2021-10-04T18:02:51.574604Z 0 [Note] InnoDB: Using Linux native AIO
2021-10-04T18:02:51.574943Z 0 [Note] InnoDB: Number of pools: 1
2021-10-04T18:02:51.575128Z 0 [Note] InnoDB: Using CPU crc32 instructions
2021-10-04T18:02:51.576528Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
2021-10-04T18:02:51.588727Z 0 [Note] InnoDB: Completed initialization of buffer pool
2021-10-04T18:02:51.590618Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
2021-10-04T18:02:51.602149Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
2021-10-04T18:02:51.610841Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2021-10-04T18:02:51.610913Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2021-10-04T18:02:51.636128Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2021-10-04T18:02:51.637092Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
2021-10-04T18:02:51.637107Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
2021-10-04T18:02:51.637540Z 0 [Note] InnoDB: Waiting for purge to start
2021-10-04T18:02:51.687831Z 0 [Note] InnoDB: 5.7.35 started; log sequence number 12665889
2021-10-04T18:02:51.688124Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2021-10-04T18:02:51.688324Z 0 [Note] Plugin 'FEDERATED' is disabled.
2021-10-04T18:02:51.691167Z 0 [Note] InnoDB: Buffer pool(s) load completed at 211004 18:02:51
2021-10-04T18:02:51.693916Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
2021-10-04T18:02:51.693931Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
2021-10-04T18:02:51.693935Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:51.693957Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
2021-10-04T18:02:51.694792Z 0 [Warning] CA certificate ca.pem is self signed.
2021-10-04T18:02:51.694855Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
2021-10-04T18:02:51.695323Z 0 [Note] Server hostname (bind-address): '*'; port: 3306
2021-10-04T18:02:51.695483Z 0 [Note] IPv6 is available.
2021-10-04T18:02:51.695530Z 0 [Note]   - '::' resolves to '::';
2021-10-04T18:02:51.695565Z 0 [Note] Server socket created on IP: '::'.
2021-10-04T18:02:51.699530Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2021-10-04T18:02:51.708497Z 0 [Note] Event Scheduler: Loaded 0 events
2021-10-04T18:02:51.708864Z 0 [Note] mysqld: ready for connections.
Version: '5.7.35'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
```
- tabel aanmaken en data meegeven

```console
mysql> CREATE TABLE Catalog(CatalogId INTEGER PRIMARY KEY,Journal VARCHAR(25),Publisher VARCHAR(25),Edition VARCHAR(25),Title VARCHAR(45),Author VARCHAR(25));
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO Catalog VALUES('1','Oracle Magazine','Oracle Publishing','November December 2013','Engineering as a Service','David A. Kelly');
Query OK, 1 row affected (0.04 sec)
```
- show tables

```console
mysql> show tables;
+-----------------+
| Tables_in_appdb |
+-----------------+
| Catalog         |
+-----------------+
1 row in set (0.00 sec)

```

- show catalog

```console
mysql> select * from Catalog;
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
| CatalogId | Journal         | Publisher         | Edition                | Title                    | Author         |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
|         1 | Oracle Magazine | Oracle Publishing | November December 2013 | Engineering as a Service | David A. Kelly |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
1 row in set (0.00 sec)

```

- stop and start db and check data
```console
vagrant@dockerlab:~$ docker stop db
db
vagrant@dockerlab:~$ docker start db
db

vagrant@dockerlab:~$ docker exec -it db mysql -pletmein appdb

mysql> select * from Catalog;
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
| CatalogId | Journal         | Publisher         | Edition                | Title                    | Author         |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
|         1 | Oracle Magazine | Oracle Publishing | November December 2013 | Engineering as a Service | David A. Kelly |
+-----------+-----------------+-------------------+------------------------+--------------------------+----------------+
1 row in set (0.00 sec)

```
### Deel 1.4 Custom images

- build container image

```console
vagrant@dockerlab:/vagrant/labs/static-website$ docker image ls
REPOSITORY               TAG           IMAGE ID       CREATED         SIZE
local                    static-site   512df4976a66   5 seconds ago   7.24MB
mysql                    5.7           9f35042c6a98   6 days ago      448MB
portainer/portainer-ce   latest        8377e6877145   11 days ago     251MB
alpine                   latest        14119a10abf4   5 weeks ago     5.6MB
tutum/hello-world        latest        31e17b0746e4   5 years ago     17.8MB
```
- after container check curl
```console
vagrant@dockerlab:/vagrant/labs/static-website$ curl http://localhost:8080/
<html>
    <head>
        <title>Success!</title>
        <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;900&display=swap" rel="stylesheet">
        <style>
        table, th, td {
          border-bottom: 1px solid black;
          padding: 10px;
          border-collapse: collapse;
          text-align: center;
        }
        .center {
          margin-left: auto;
          margin-right: auto;
        }
        h1 {
          text-align: center;
          font-size: 50px;
        }
        p {
          text-align: center;
        }
        .center {
          display: block;
          margin-left: auto;
          margin-right: auto;
          width: 50%;
        }
        * {
          font-family: Montserrat;
          font-size: 20px;
        }
        </style>
    </head>
    <body>
        <h1>Hello world</h1>
        <p>It works!</p>

        <img alt="Works on my machine. OPS problem now."
             src="works-on-my-machine-ops-problem-now.jpg"
             class="center"/>
    </body>
```

![custom container image, 4 october 2021](img/Cringe.PNG?raw=true)

### Deel 1.5 Layered file system

- checksum

```console
vagrant@dockerlab:~$ docker image inspect alpine:latest | jq ".[]|.RootFS.Layers"
[
  "sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68"
]
```

- split up run

  ```console
  FROM alpine:latest
  LABEL description="This example Dockerfile installs NGINX."
  RUN apk add --update nginx
  RUN rm -rf /var/cache/apk/*
  RUN mkdir -p /tmp/nginx/
  COPY files/nginx.conf /etc/nginx/nginx.conf
  COPY files/default.conf /etc/nginx/conf.d/default.conf
  ADD files/site-contents.tar.bz2 /usr/share/nginx/
  EXPOSE 80/tcp
  ENTRYPOINT ["nginx"]
  CMD ["-g", "daemon off;"]
  ```

- rebuild and rename

  ```console
  vagrant@dockerlab:/vagrant/labs/static-website$ docker image build --tag local:static-site-2 .
  Sending build context to Docker daemon  417.8kB
  Step 1/11 : FROM alpine:latest
   ---> c059bfaa849c
  Step 2/11 : LABEL description="This example Dockerfile installs NGINX."
   ---> Running in 44cb5886000a
  Removing intermediate container 44cb5886000a
   ---> f497c2ec498e
  Step 3/11 : RUN apk add --update nginx
   ---> Running in 571aac6b98cd
  fetch https://dl-cdn.alpinelinux.org/alpine/v3.15/main/x86_64/APKINDEX.tar.gz
  fetch https://dl-cdn.alpinelinux.org/alpine/v3.15/community/x86_64/APKINDEX.tar.gz
  (1/2) Installing pcre (8.45-r1)
  (2/2) Installing nginx (1.20.2-r0)
  Executing nginx-1.20.2-r0.pre-install
  Executing nginx-1.20.2-r0.post-install
  Executing busybox-1.34.1-r3.trigger
  OK: 7 MiB in 16 packages
  Removing intermediate container 571aac6b98cd
   ---> 79480ea5598a
  Step 4/11 : RUN rm -rf /var/cache/apk/*
   ---> Running in 36b2d181212e
  Removing intermediate container 36b2d181212e
   ---> eedab169d7d6
  Step 5/11 : RUN mkdir -p /tmp/nginx/
   ---> Running in 0d20b4a774aa
  Removing intermediate container 0d20b4a774aa
   ---> ef5278a20f4e
  Step 6/11 : COPY files/nginx.conf /etc/nginx/nginx.conf
   ---> 1f87027adbb0
  Step 7/11 : COPY files/default.conf /etc/nginx/conf.d/default.conf
   ---> f9a0b414aedc
  Step 8/11 : ADD files/site-contents.tar.bz2 /usr/share/nginx/
   ---> 062188453f24
  Step 9/11 : EXPOSE 80/tcp
   ---> Running in 91fbca4cc409
  Removing intermediate container 91fbca4cc409
   ---> d2de179d6eb7
  Step 10/11 : ENTRYPOINT ["nginx"]
   ---> Running in 52089df12789
  Removing intermediate container 52089df12789
   ---> 088f4e1416c0
  Step 11/11 : CMD ["-g", "daemon off;"]
   ---> Running in 49850421d31d
  Removing intermediate container 49850421d31d
   ---> 66c764fb2e39
  Successfully built 66c764fb2e39
  Successfully tagged local:static-site-2
  ```

- change index.html

  ```console
  <html>
      <head>
          <title>Success!</title>
          <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;900&display=swap" rel="stylesheet">
          <style>
          table, th, td {
            border-bottom: 1px solid black;
            padding: 10px;
            border-collapse: collapse;
            text-align: center;
          }
          .center {
            margin-left: auto;
            margin-right: auto;
          }
          h1 {
            text-align: center;
            font-size: 50px;
          }
          p {
            text-align: center;
          }
          .center {
            display: block;
            margin-left: auto;
            margin-right: auto;
            width: 50%;
          }
          * {
            font-family: Montserrat;
            font-size: 20px;
          }
          </style>
      </head>
      <body style="background-color:powderblue;">
          <h1>Hello world</h1>
          <p>It works!</p>
  
          <img alt="Works on my machine. OPS problem now."
               src="works-on-my-machine-ops-problem-now.jpg"
               class="center"/>
      </body>
  </html>
  ```

- run 

  ```console
  vagrant@dockerlab:~$ docker run local:static-site-2
  ^Cvagrant@dockerlab:~$ docker run -d local:static-site-2
  6912669c80ed3eaca746ac20b05cb673ad041565a6ee93057e4578cc19705d84
  vagrant@dockerlab:~$ docker ps
  CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS             PORTS                                                      NAMES
  6912669c80ed   local:static-site-2      "nginx -g 'daemon of…"   7 seconds ago    Up 6 seconds       80/tcp                                                     elegant_mclaren
  473eacabc798   local:static-site        "nginx -g 'daemon of…"   13 minutes ago   Up 13 minutes      80/tcp                                                     heuristic_dijkstra
  ceb72dda54f6   tutum/hello-world        "/bin/sh -c 'php-fpm…"   34 minutes ago   Up 34 minutes      0.0.0.0:49153->80/tcp, :::49153->80/tcp                    helloapp
  05f853e921e6   portainer/portainer-ce   "/portainer"             8 weeks ago      Up About an hour   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
  ```

### Deel 1.6 Docker compose

- todo app

  ```console
  vagrant@dockerlab:~$ cd /vagrant/labs/todo-app
  vagrant@dockerlab:/vagrant/labs/todo-app$ tree
  .
  ├── docker-compose.yml
  ├── Dockerfile
  ├── package.json
  ├── README.md
  ├── spec
  │   ├── persistence
  │   │   └── sqlite.spec.js
  │   └── routes
  │       ├── addItem.spec.js
  │       ├── deleteItem.spec.js
  │       ├── getItems.spec.js
  │       └── updateItem.spec.js
  ├── src
  │   ├── index.js
  │   ├── persistence
  │   │   ├── index.js
  │   │   ├── mysql.js
  │   │   └── sqlite.js
  │   ├── routes
  │   │   ├── addItem.js
  │   │   ├── deleteItem.js
  │   │   ├── getItems.js
  │   │   └── updateItem.js
  │   └── static
  │       ├── css
  │       │   ├── bootstrap.min.css
  │       │   ├── font-awesome
  │       │   │   ├── all.min.css
  │       │   │   ├── fa-brands-400.eot
  │       │   │   ├── fa-brands-400.svg#fontawesome
  │       │   │   ├── fa-brands-400.ttf
  │       │   │   ├── fa-brands-400.woff
  │       │   │   ├── fa-brands-400.woff2
  │       │   │   ├── fa-regular-400.eot
  │       │   │   ├── fa-regular-400.svg#fontawesome
  │       │   │   ├── fa-regular-400.ttf
  │       │   │   ├── fa-regular-400.woff
  │       │   │   ├── fa-regular-400.woff2
  │       │   │   ├── fa-solid-900.eot
  │       │   │   ├── fa-solid-900.svg#fontawesome
  │       │   │   ├── fa-solid-900.ttf
  │       │   │   ├── fa-solid-900.woff
  │       │   │   └── fa-solid-900.woff2
  │       │   └── styles.css
  │       ├── index.html
  │       └── js
  │           ├── app.js
  │           ├── babel.min.js
  │           ├── react-bootstrap.js
  │           ├── react-dom.production.min.js
  │           └── react.production.min.js
  └── yarn.lock
  
  10 directories, 42 files
  ```

- docker build

  ```console
  vagrant@dockerlab:/vagrant/labs/todo-app$ docker image build --tag todo-app .
  Sending build context to Docker daemon  63.32MB
  Step 1/6 : FROM node:12-alpine
   ---> 2f014773d54a
  Step 2/6 : RUN apk add --no-cache python g++ make
   ---> Running in 2239d1d31c37
  fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/APKINDEX.tar.gz
  fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/community/x86_64/APKINDEX.tar.gz
  ERROR: unable to select packages:
    python (no such package):
      required by: world[python]
  The command '/bin/sh -c apk add --no-cache python g++ make' returned a non-zero code: 1
  ```

- add python

  ```console
  # syntax=docker/dockerfile:1
  FROM node:12-alpine
  RUN echo -e "http://nl.alpinelinux.org/alpine/v3.5/main\nhttp://nl.alpinelinux.org/alpine/v3.5/community" > /etc/apk/repositories
  RUN apk add --no-cache python g++ make
  WORKDIR /app
  COPY . .
  RUN yarn install --production
  CMD ["node", "src/index.js"]
  ```

- build

  ```console
  vagrant@dockerlab:/vagrant/labs/todo-app$ docker image build --tag todo-app .
  Sending build context to Docker daemon  63.32MB
  Step 1/7 : FROM node:12-alpine
   ---> 2f014773d54a
  Step 2/7 : RUN echo -e "http://nl.alpinelinux.org/alpine/v3.5/main\nhttp://nl.alpinelinux.org/alpine/v3.5/community" > /etc/apk/repositories
   ---> Running in 41425065a701
  Removing intermediate container 41425065a701
   ---> 05e0ec0d46a0
  Step 3/7 : RUN apk add --no-cache python g++ make
   ---> Running in 7b394f696934
  fetch http://nl.alpinelinux.org/alpine/v3.5/main/x86_64/APKINDEX.tar.gz
  fetch http://nl.alpinelinux.org/alpine/v3.5/community/x86_64/APKINDEX.tar.gz
  (1/34) Purging ssl_client (1.33.1-r6)
  (2/34) Downgrading musl (1.2.2-r3 -> 1.1.15-r8)
  (3/34) Installing libressl2.4-libcrypto (2.4.4-r0)
  (4/34) Installing libressl2.4-libssl (2.4.4-r0)
  (5/34) Downgrading apk-tools (2.12.7-r0 -> 2.6.10-r0)
  (6/34) Downgrading libstdc++ (10.3.1_git20210424-r2 -> 6.2.1-r1)
  (7/34) Installing binutils-libs (2.27-r1)
  (8/34) Installing binutils (2.27-r1)
  (9/34) Installing gmp (6.1.1-r0)
  (10/34) Installing isl (0.17.1-r0)
  (11/34) Installing libgomp (6.2.1-r1)
  (12/34) Installing libatomic (6.2.1-r1)
  (13/34) Installing pkgconf (1.0.2-r0)
  (14/34) Installing mpfr3 (3.1.5-r0)
  (15/34) Installing mpc1 (1.0.3-r0)
  (16/34) Installing gcc (6.2.1-r1)
  (17/34) Installing musl-dev (1.1.15-r8)
  (18/34) Installing libc-dev (0.7-r1)
  (19/34) Installing g++ (6.2.1-r1)
  (20/34) Installing make (4.2.1-r0)
  (21/34) Installing libbz2 (1.0.6-r5)
  (22/34) Installing expat (2.2.0-r1)
  (23/34) Installing libffi (3.2.1-r2)
  (24/34) Installing gdbm (1.12-r0)
  (25/34) Installing ncurses-terminfo-base (6.0_p20171125-r1)
  (26/34) Installing ncurses-terminfo (6.0_p20171125-r1)
  (27/34) Installing ncurses-libs (6.0_p20171125-r1)
  (28/34) Installing readline (6.3.008-r4)
  (29/34) Installing sqlite-libs (3.15.2-r2)
  (30/34) Installing python2 (2.7.15-r0)
  (31/34) Purging libretls (3.3.3p1-r2)
  (32/34) Purging ca-certificates-bundle (20191127-r5)
  (33/34) Purging libssl1.1 (1.1.1l-r0)
  (34/34) Purging libcrypto1.1 (1.1.1l-r0)
  Executing busybox-1.33.1-r6.trigger
  OK: 206 MiB in 37 packages
  Removing intermediate container 7b394f696934
   ---> 10ccaf51a564
  Step 4/7 : WORKDIR /app
   ---> Running in 2c85d3ecc5a8
  Removing intermediate container 2c85d3ecc5a8
   ---> f95c626f5da4
  Step 5/7 : COPY . .
   ---> 53409f2c7943
  Step 6/7 : RUN yarn install --production
   ---> Running in eb352956224b
  yarn install v1.22.15
  [1/4] Resolving packages...
  [2/4] Fetching packages...
  info fsevents@1.2.9: The platform "linux" is incompatible with this module.
  info "fsevents@1.2.9" is an optional dependency and failed compatibility check. Excluding it from installation.
  [3/4] Linking dependencies...
  [4/4] Building fresh packages...
  Done in 22.05s.
  Removing intermediate container eb352956224b
   ---> 5420a1e7ece5
  Step 7/7 : CMD ["node", "src/index.js"]
   ---> Running in 236978ef27ec
  Removing intermediate container 236978ef27ec
   ---> 63bb0bb942de
  Successfully built 63bb0bb942de
  Successfully tagged todo-app:latest
  ```

- run

  ```console
  vagrant@dockerlab:/vagrant/labs/todo-app$ docker run -d -p 3000:3000 todo-app
  352f96aba6450220939491c85ecc687712cabea6d66775c3164441680c157ac5
  vagrant@dockerlab:/vagrant/labs/todo-app$ docker ps
  CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS          PORTS                                                      NAMES
  352f96aba645   todo-app                 "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds    0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                  inspiring_ganguly
  05f853e921e6   portainer/portainer-ce   "/portainer"             8 weeks ago     Up 37 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
  ```

  ![todo-app, 4 october 2021](img/TodoApp.PNG?raw=true)

  

- docker compose

  ```console
  vagrant@dockerlab:/vagrant/labs/todo-app$ docker-compose up -d
  Creating network "todo-app_default" with the default driver
  Creating volume "todo-app_todo-mysql-data" with default driver
  Creating todo-app_db_1  ... done
  Creating todo-app_app_1 ... done
  ```

  

## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
