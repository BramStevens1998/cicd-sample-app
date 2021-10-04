# Lab Report: Lab 1 : Container virtulisation

## Login creds
	admin
	oud passwd bd
## Setup
	vagrant up dockerlab
	vagrant provision dockerlab

## Lab
### Deel 1.1
- Web UI up and running

![Home, 4 october 2021](img/HOME.PNG?raw=true)

- Check status of the Docker engine

![Stat Docker, 4 october 2021](img/StatDocker.PNG?raw=true)

- Check network TCP server ports

![TCP ports, 4 october 2021](img/TCPports.PNG?raw=true)

- List running Docker containers

![Docker containers, 4 october 2021](img/DockerContainers.PNG?raw=true)

- List Docker images

![DockerImages, 4 october 2021](img/DockerImages.PNG?raw=true)

### Deel 1.2
 - hostname container

![Hostname, 4 october 2021](img/Hostname.PNG?raw=true)

 - ip address container

![Container IP, 4 october 2021](img/ContainerIP.PNG?raw=true)

 - exit shell, still up?

 ![Alpine up, 4 october 2021](img/AlpineRunning.PNG?raw=true)

 - Docker image

 ![Hello app, 4 october 2021](img/HelloApp.PNG?raw=true)

### deel 1.3

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




## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
