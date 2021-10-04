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


### deel 1.3

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
### deel 1.4

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


## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
