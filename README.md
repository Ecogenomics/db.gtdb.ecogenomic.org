# db.gtdb.ecogenomic.org

This repository contains the infrastructure required to
serve the GTDB database: `db.gtdb.ecogenomic.org`

## Contributing

This project uses [Semantic Versioning](http://semver.org/) and [Conventional Commits](https://conventionalcommits.org/)
to automatically generate release notes using [Semantic Release](https://semantic-release.gitbook.io/semantic-release/).

Please ensure that your commits are property formatted.

## Setup

If you are installing this on a fresh VM, then you will need to follow these setup guides:

* [Virtual machine](doc/vm-setup.md)
* [PostgreSQL](doc/db-setup.md)

## Controlling services

The following commands will be available to you when in the current directory:

```shell
cd /var/db.gtdb.ecogenomic.org
sudo docker-compose up -d  # start
sudo docker-compose down   # stop
sudo docker-compose logs   # logs
```
