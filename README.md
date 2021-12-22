# hive-metastore-docker
Containerized Apache Hive Metastore for horizontally scalable Hive Metastore deployments

The `hive-metastore` image is stored on Docker Hub in the [rtdl/`hive-metastore` repository](https://hub.docker.com/repository/docker/rtdl/hive-metastore.

## Use
* This image is not interactive and has no default ENTRYPOINT. You must use the `entrypoint` option along with a corresponding shell script and `volumes` to load scripts to execute.
* A PostgreSQL-compatible database is required to run the container. Define your connection credentials in `conf/metastore-site.xml` and use the 'volumes' option to load your `metastore-site.xml` file to `/opt/apache-hive-metastore-{version}-bin/conf/metastore-site.xml`.
* This image opens port `9083`. Use this port to connect to your Hive Metastore instance.

**Docker Run example**
```
docker run --name hive-metastore \
-v ${PWD}/scripts/entrypoint-run.sh:/entrypoint.sh \
-v ${PWD}/conf/metastore-site.xml:/opt/apache-hive-metastore-3.1.2-bin/conf/metastore-site.xml \
--entrypoint sh \
rtdl/hive-metastore:latest \
-c "chown hive:hive /entrypoint.sh && chmod +x /entrypoint.sh && sh -c /entrypoint.sh"
```

**Docker Compose example**
```
catalog:
  image: rtdl/hive-metastore:latest
  container_name: hive-metastore
  ports:
    - 9083:9083
  volumes:
    - ./scripts/entrypoint-run.sh:/entrypoint.sh
    - ./conf/metastore-site.xml:/opt/apache-hive-metastore-3.1.2-bin/conf/metastore-site.xml
  entrypoint: sh -c "chown hive:hive /entrypoint.sh && chmod +x /entrypoint.sh && sh -c /entrypoint.sh"
```

## Build
If you want to build the image yourself, clone the repo and run `docker build -t rtdl/hive-metastore:latest .`.
