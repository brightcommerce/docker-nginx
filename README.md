# Docker Nginx

Dockerfile to build Nginx v1.6.0 container image.

## Table of Contents

- [Version](#version)
- [Installation](#installation)
- [Configuration](#configuration)
    - [Volumes](#volumes)
    - [Ports](#ports)
- [How To Use](#how-to-use)
    - [Shell Access](#shell-access)
    - [Daemonized Service](#daemonized-service)
- [Upgrading](#upgrading)
- [Copyright](#copyright)

## Version

The current release (1.6.0.20140915) contains scripts to install Nginx v1.6.0, and uses the Brightcommerce Ubuntu 14.04 base image. Our version numbers will reflect the version of Nginx being installed.

## Installation

Pull the latest version of the image from the Docker Index. This is the recommended method of installation as it is easier to update the image in the future. These builds are performed by the **Docker Trusted Build** service.

``` bash
docker pull brightcommerce/nginx:latest
```

or specify a tagged version:

``` bash
docker pull brightcommerce/nginx:1.6.0.20140915
```

Alternately you can build the image yourself:

``` bash
git clone https://github.com/brightcommerce/docker-nginx.git
cd docker-nginx
docker build -t="$USER/nginx" .
```

## Configuration

This image uses volumes and environment variables to control the nginx server configuration.

### Volumes

* `/etc/nginx/sites-available`: Site configurations
* `/usr/share/nginx/html`: HTML for the default site, and 
* `/etc/nginx/conf.d`: Change sites configurations using it.
* `/var/log/nginx`: Access log from here.

Pass with `-v` docker option. Don't forget to use absolute path here.

### Ports

This installation exposes port `80`.

## How To Use

### Shell Access

The following Docker `run` command above will start a container and give you a shell. Don't forget to start the service running the `startup &` script:

``` bash
$ docker run -p 80:80 -i \
-v `pwd`/volumes/sites-available:/etc/nginx/sites-available \
-v `pwd`/volumes/html:/usr/share/nginx/html \
-v `pwd`/volumes/conf.d:/etc/nginx/conf.d \
-v `pwd`/volumes/log:/var/log/nginx \
-t brightcomemrce/nginx:latest /bin/bash
```

### Daemonized Service

The following command above will start a container and return its' ID:

``` bash
$ docker run --name nginx -p 80:80 -d \
-v `pwd`/volumes/sites-available:/etc/nginx/sites-available \
-v `pwd`/volumes/html:/usr/share/nginx/html \
-v `pwd`/volumes/conf.d:/etc/nginx/conf.d \
-v `pwd`/volumes/log:/var/log/nginx \
-t brightcommerce/nginx:latest
```

## Upgrading

To upgrade to newer releases, simply follow this 3 step upgrade procedure.

- **Step 1**: Stop the currently running image:

``` bash
docker stop nginx
```

- **Step 2**: Update the docker image:

``` bash
docker pull brightcommerce/nginx:latest
```

- **Step 3**: Start the image:

``` bash
docker run --name nginx -d [OPTIONS] brightcommerce/nginx:latest
```

## Copyright

Copyright 2014 Brightcommerce, Inc.
