# hive-metastore-docker
Containerized Apache Hive Metastore for horizontally scalable Hive Metastore deployments 
backed by a PostgreSQL-compatible database.

The `hive-metastore` image is stored on Docker Hub in the [rtdl/hive-metastore repository](https://hub.docker.com/repository/docker/rtdl/hive-metastore).

## How to Use
To get a persistent Apache Hive Metastore instance running in a container backed by a 
PostgreSQL-compatible database (all files stored in `storage/` folder):
  1.  Run `docker compose -f docker-compose.init.yml up -d`.
      * **Note:** This configuration should be fault-tolerant, but if any containers or 
      processes fail when running this, run `docker compose -f docker-compose.init.yml down` 
      and retry.
  2.  After containers `rtdl_catalog-db-init` and `rtdl_catalog-init` exit and complete with 
      `EXITED (0)`, kill and delete the rtdl container set by running 
      `docker compose -f docker-compose.init.yml down`
3. Run `docker compose up -d` every time after.
    * `docker compose down` to stop.

**Note:** To start from scratch, first run the below commands from the repo's root folder.
```
% rm -rf storage/
``` 

## Notes
  * This image is not interactive and has no default ENTRYPOINT. You must use the `entrypoint` 
option along with a corresponding shell script and `volumes` to load scripts to execute.
  * A PostgreSQL-compatible database is required to run the container. Define your connection 
credentials in `conf/metastore-site.xml` and use the 'volumes' option to load your `metastore-site.xml` file to `/opt/apache-hive-metastore-{version}-bin/conf/metastore-site.xml`.
  * This image opens port `9083`. Use this port to connect to your Hive Metastore instance.
