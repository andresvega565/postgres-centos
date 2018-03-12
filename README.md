
### Installation

1. Install [Docker](https://www.docker.com/).

2. You can download automated build from public Docker Hub Registry:

``` docker pull  ndongelo/centos-postgresql ```

### Create and running a container

**Create container:**

(Not recommended for production use)

``` docker create -it -p 5432:5432 --name postgresql94  ndongelo/centos-postgresql ```

**Start container:**

``` docker start postgresql94 ```


**Another way to start a postgresql container:**

``` docker run -d -p 5432:5432 --name postgresql94  ndongelo/centos-postgresql ```

### Connection methods:

**PostgreSQL client:**

`docker exec -it postgresql94 psql`

**Bash:**

`docker exec -it postgresql94 bash`


### Creating a database and username

You can create a postgresql database and superuser at launch. Use `DB_NAME`, `DB_USER` and `DB_PASS` variables.

```
docker create -it -p 5432:5432 --name postgresql --env 'DB_USER=YOUR_USERNAME' --env 'DB_PASS=YOUR_PASSWORD' --env 'DB_NAME=YOUR_DATABASE'  ndongelo/centos-postgresql

```
 
If you don't set DB_PASS variable, an automatic password is generated for the PostgreSQL database user. Check to stdout/stderr log of container created:

```
docker run -d -p 5432:5432 --name postgresql94 --env 'DB_USER=YOUR_USERNAME' --env 'DB_NAME=YOUR_DATABASE'  ndongelo/centos-postgresql
docker logs postgresql94
```

The output:

```
...
WARNING: 
No password specified for "YOUR_USERNAME". Generating one
Password for "YOUR_USERNAME" created as: "aich3aaH0yiu"
...
```

To connect to newly created postgresql container:

`docker exec -it postgresql94 psql -U YOUR_USERNAME`

Another way to connect to postgresql container with your newly created user:

```
psql -U YOUR_USERNAME -h $(docker inspect --format {{.NetworkSettings.IPAddress}} postgresql94)
```


### Upgrading

Stop the currently running image:

``` docker stop postgresql94 ```


Update the docker image:

``` docker pull ndongelo/centos-postgresql ```
