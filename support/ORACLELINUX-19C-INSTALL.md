
# ORACLELINUX 19C INSTALL

> The version used in this document for Oracle is 19.3.0.0.0

### Requirements

- 20GB free space
- Docker
- Docker Compose
- LINUX.X64_193000_db_home.zip (this file can be downloaded in https://www.oracle.com/database/technologies/oracle-database-software-downloads.html)

### Reference

> The complete installation using docker + docker-compose is available on the following link
> https://github.com/huntercodexs/docker-series/tree/databases?tab=readme-ov-file#oracle

Please note that this installation require an environment that use a docker container and don't allow you 
to install in your machine as a database resource like Mysql, it's a simple resource to get one instance oraclelinux 
for development not production.

As a alternative it's possible to create a VM using the ISO file available at Yum Oracle accessible through the 
following page https://yum.oracle.com/oracle-linux-isos.html
