# Getting started to use pgbouncer
This [repository](https://github.com/kongyew/greenplum-pgbouncer) demonstrates how to configure Greenplum with pgbouncer. You can complete this example in 10 minutes.

# Table of Contents
1. [Pre-requisites](#Pre-requisites)
2. [Start Docker-compose](#Start-Docker-compose)
3. [Setup pgbouncer](#setup-pgbouncer)
4. [Use pgbouncer](#Use-pgbouncer)

## Pre-requisites:
- [docker-compose](http://docs.docker.com/compose)
- [GPDB 5.x docker image](https://hub.docker.com/r/kochanpivotal/)

## Start Docker-compose
Once you have cloned this repository, you can run the command  `./runDocker.sh -t usecase1 -c up`, in order to start  Greenplum docker instance.

The assumption: docker and docker-compose are already installed on your machine.

### Run command to start both Greenplum
```
$ ./runDocker.sh -t usecase1 -c up
Recreating gpdbsne ... done
gpdbsne     | /etc/sysconfig/run-parts
gpdbsne     | /usr/bin/run-parts
gpdbsne     | Running /docker-entrypoint.d
gpdbsne     | /docker-entrypoint.d/startInit.sh:
gpdbsne     |
gpdbsne     | init is running
gpdbsne     | /docker-entrypoint.d/startSSH.sh:
```

### How to access Greenplum docker instance:
You can use this command `docker exec -it gpdbsne bin/bash` to access Greenplum docker instance.

For example:
```
$ docker ps
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS              PORTS                                                                                             NAMES
22c87a7bc39d        kochanpivotal/gpdb5-pxf      "/docker-entrypoint.…"   4 minutes ago       Up 4 minutes        0.0.0.0:5005->5005/tcp, 0.0.0.0:5010->5010/tcp, 0.0.0.0:5432->5432/tcp, 0.0.0.0:40000-40002->40000-40002/tcp, 0.0.0.0:9022->22/tcp                                                                               gpdbsne

root@gpdbsne:/#
```

## Setup pgbouncer
Once you have access to Greenplum docker instance, you can configure pgbouncer.

1. Start GPDB instance:
Use the command 'startGPDB.sh'
```
root@gpdbsne# startGPDB.sh
SSHD isn't running
 * Starting OpenBSD Secure Shell server sshd                             [ OK ]
SSHD is running...
20180419:21:15:09:000094 gpstart:gpdbsne:gpadmin-[INFO]:-Starting gpstart with args: -a
20180419:21:15:09:000094 gpstart:gpdbsne:gpadmin-[INFO]:-Gathering information and validating the environment...
...
20180419:21:15:18:000247 gpstart:gpdbsne:gpadmin-[INFO]:-Have lock file /tmp/.s.PGSQL.5432 and a process running on port 5432
20180419:21:15:18:000247 gpstart:gpdbsne:gpadmin-[ERROR]:-gpstart error: Master instance process running
```
2. Use gpadmin user.
```
[root@gpdbsne /]# su - gpadmin
Last login: Wed May 30 23:51:49 UTC 2018 on pts/0
[gpadmin@gpdbsne ~]$
```
3. Create pgbouncer.ini with these settings below.
```
[databases]
postgres = host=127.0.0.1 port=5432 dbname=postgres
gpadmin = host=127.0.0.1 port=5432 dbname=gpadmin
[pgbouncer]
pool_mode = session
listen_port = 6543
listen_addr = 127.0.0.1
auth_type = md5
auth_file = users.txt
logfile = pgbouncer.log
pidfile = pgbouncer.pid
admin_users = gpadmin
```
4.  Create an authentication file such as users.txt
For example: use these data
```
"gpadmin" "pivotal"
```
5. Launch pgbouncer by using this command `$GPHOME/bin/pgbouncer -d pgbouncer.ini`.
```
gpadmin@gpdbsne ~]$ $GPHOME/bin/pgbouncer -d pgbouncer.ini
2018-06-05 00:17:18.094 421 LOG File descriptor limit: 65536 (H:65536), max_client_conn: 100, max fds possible: 150
```
You can verify pgbouncer by running `ps -aux | grep pgbouncer`.
6. Use psql to access pgbouncer.
```
gpadmin@gpdbsne ~]$  psql -p 6543 -U gpadmin postgres
```

# Reference:
* [gpbackup](https://gpdb.docs.pivotal.io/580/utility_guide/admin_utilities/gpbackup.html)
* [Greenplum product](https://pivotal.io/pivotal-greenplum)
* [Greenplum documentations](https://https://gpdb.docs.pivotal.io/)
* [Greenplum documentations](https://https://gpdb.docs.pivotal.io/)
https://github.com/greenplum-db/gpbackup-s3-plugin
