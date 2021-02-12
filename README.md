# My changes

As of the fact, this work is not whole mine, I would like to mention what I have exactly done here.

I have extended nginx configuration for large file support: bigger then default POST max size as well as max packet size.
This was just a little change.

The bigger thing was to properly configure php-fpm container, thus I mean php7.4 with Sf4.4 to use MSSQL server.
Used image is from official Microsoft DockerHub account. 

I had to experiment with both drivers for SQL Server to get it working. I had a hard time working on it, thus I want this to pu accessible to anybody who finds this repo, while being in need.

Ofc, any contributions are in order.

Greetings.

Software used:
- tested for Sf4.4 + Sonata3,
- MSSQL v.17,
- nginx 1.19,
- ELK as below, nothing has been changed,
- php-fpm (fpm-fcgi) for php7.4


# This is original README of the project accessible [here](https://github.com/eko/docker-symfony).
docker-symfony
==============


This is a complete stack for running Symfony 4 (latest version: Flex) into Docker containers using docker-compose tool.

# Installation

First, clone this repository:

```bash
$ git clone https://github.com/eko/docker-symfony.git
```

Next, put your Symfony application into `symfony` folder and do not forget to add `symfony.localhost` in your `/etc/hosts` file.

Make sure you adjust `database_host` in `parameters.yml` to the database container alias "db" (for Symfony < 4)
Make sure you adjust `DATABASE_URL` in `env` to the database container alias "db" (for Symfony >= 4)

Then, run:

```bash
$ docker-compose up
```

You are done, you can visit your Symfony application on the following URL: `http://symfony.localhost` (and access Kibana on `http://symfony.localhost:81`)

_Note :_ you can rebuild all Docker images by running:

```bash
$ docker-compose build
```

# How it works?

Here are the `docker-compose` built images:

* `db`: This is the MySQL database container (can be changed to postgresql or whatever in `docker-compose.yml` file),
* `php`: This is the PHP-FPM container including the application volume mounted on,
* `nginx`: This is the Nginx webserver container in which php volumes are mounted too,
* `elasticsearch`: This is the Elasticsearch server used to store our web server and application logs,
* `logstash`: This is the Logstash tool from Elastic Stack that allows to read logs and send them into our Elasticsearch server,
* `kibana`: This is the Kibana UI that is used to render logs and create beautiful dashboards. 

This results in the following running containers:

```bash
> $ docker-compose ps
             Name                           Command               State                 Ports
-----------------------------------------------------------------------------------------------------------
mysql                            docker-entrypoint.sh --def ...   Up      0.0.0.0:3306->3306/tcp, 33060/tcp
elasticsearch                    /usr/local/bin/docker-entr ...   Up      0.0.0.0:9200->9200/tcp, 9300/tcp
kibana                           /usr/local/bin/dumb-init - ...   Up      0.0.0.0:81->5601/tcp
logstash                         /usr/local/bin/docker-entr ...   Up      5044/tcp, 9600/tcp
nginx                            nginx                            Up      443/tcp, 0.0.0.0:80->80/tcp
php-fpm                          php-fpm7 -F                      Up      0.0.0.0:9000->9001/tcp
```

# Read logs

You can access Nginx and Symfony application logs in the following directories on your host machine:

* `logs/nginx`
* `logs/symfony`

# Use Kibana!

You can also use Kibana to visualize Nginx & Symfony logs by visiting `http://symfony.localhost:81`.

# Use xdebug!

Configure your IDE to use port 5902 for XDebug.
Docker versions below 18.03.1 don't support the Docker variable `host.docker.internal`.  
In that case you'd have to swap out `host.docker.internal` with your machine IP address in php-fpm/xdebug.ini.

# Code license

You are free to use the code in this repository under the terms of the 0-clause BSD license. LICENSE contains a copy of this license.
