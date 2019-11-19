# docker-db2

## :no_entry: Deprecated

This repository is deprecated and has been replaced by
[docker-ibm-db2](https://github.com/Senzing/docker-ibm-db2).

[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)

## Overview

This Dockerfile is a wrapper over the DB2 command-line interface,
a.k.a. [Command line processor](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0010409.html).

### Contents

1. [Create Docker container](#create-docker-container)
1. [Run Docker container](#run-docker-container)
1. [Start DB2 Command-line processor](start-db2-command-line-processor)

## Create docker container

```console
sudo docker build --tag senzing/db2 https://github.com/senzing/docker-db2.git
```

## Run Docker container

1. **Option #1** - Run the docker container without volumes or network.

    Identify the `db2inst1` user's password.
    Example:

    ```console
    export DB2INST1_PASSWORD=password
    ```

    Run docker container.

    ```console
    sudo docker run -it  \
      --env DB2INST1_PASSWORD=${DB2INST1_PASSWORD} \
      --env LICENSE="accept" \
      senzing/db2
    ```

1. **Option #2** - Run the docker container with volumes.

    Identify the Senzing directory.
    Default: `/opt/senzing`.
    Example:

    ```console
    export SENZING_DIR=/opt/senzing
    ```

    Run docker container.

    ```console
    sudo docker run -it  \
      --volume ${SENZING_DIR}:/opt/senzing \
      --env DB2INST1_PASSWORD=${DB2INST1_PASSWORD} \
      --env LICENSE="accept" \
      senzing/db2
    ```

1. **Option #3** - Run the docker container in a docker network.

    Identify the Docker network of the DB2 database.
    Example:

    ```console
    sudo docker network ls

    # Choose value from NAME column of docker network ls
    export DB2_NETWORK=nameofthe_network
    ```

    Run docker container.

    ```console
    sudo docker run -it  \
      --volume ${SENZING_DIR}:/opt/senzing \
      --net ${DB2_NETWORK} \
      --env DB2INST1_PASSWORD=${DB2INST1_PASSWORD} \
      --env LICENSE="accept" \
      senzing/db2
    ```

## Start DB2 Command-line processor

1. At the docker container's `/bin/bash` prompt:

    ```console
    su - db2inst1
    db2
    ```

## Example DB2 commands

1. Become `db2inst1` user. At the docker container's `/bin/bash` prompt:

    ```console
    su - db2inst1
    ```

1. Example `db2` commands:

    ```console
    db2 catalog tcpip node G2 remote senzing-db2 server 50000
    db2 terminate
    db2 list node directory
    db2 attach to G2 user db2inst1 using db2inst1
    db2 create database g2 using codeset utf-8 territory us
    db2 connect to g2 user db2inst1 using db2inst1
    db2 -tf /opt/senzing/g2/data/g2core-schema-db2-create.sql | tee /tmp/g2schema.out
    db2 connect reset
    ```
