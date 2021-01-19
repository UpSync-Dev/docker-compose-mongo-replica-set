# Running a Local Mongo Replica Set

## Contents
* (What is it?)[#what-is-it]
* (Are there any prerequisites?)[#are-there-any-prerequisites]
* (How do I run the Replica Set?)[#how-do-i-run-the-replica-set]
* (How do I access the Mongo Shells for each Instance?)[#how-do-i-access-the-mongo-shells-for-each-instance]
* (How does it work?)[#how-does-it-work]
* (Robo 3T)[#robo-3t]
* (Thanks / Further Reading)[#thanks--further-reading]

## What is it?
This `docker-compose` setup starts a local mongo replica set with 3 instances running on: 
- mongo1:30001
- mongo2:30002
- mongo3:30003

## Are there any prerequisites? 
• Docker Compose
• Docker
• The following in your `/etc/hosts` file:
```
127.0.0.1       mongo1
127.0.0.1       mongo2
127.0.0.1       mongo3
```

## How do I run the Replica Set?
Simples:
```
docker-compose up -d
```

## How do I access the Mongo Shells for each Instance?
```
docker exec -it mongo1 sh -c "mongo --port 30001"
docker exec -it mongo2 sh -c "mongo --port 30002"
docker exec -it mongo3 sh -c "mongo --port 30003"
```

## How does it work?
- Starts three instances of Mongo
- On the first instance it runs the following Mongo Shell command:
```
rs.initiate(
  {
    _id : 'my-replica-set',
    members: [
      { _id : 0, host : "mongo1:30001" },
      { _id : 1, host : "mongo2:30002" },
      { _id : 2, host : "mongo3:30003" }
    ]
  }
)
```
- This causes all 3 instances to join the replica set named `my-replica-set` and start talking to each other
- One is elected to become the `PRIMARY` and the other two become `SECONDARY` instances
- The Docker healthcheck config is used to cause the initialisation of the replica set. More info in the further reading links.

## Robo 3T
I used Robo 3T to test it locally and used the following config for the connection:

![Robo 3T Config](https://github.com/UpSync-Dev/docker-compose-mongo-replica-set/raw/main/robo-3t.png)

## Thanks / Further Reading
- [How to turn standalone MongoDB server into a replica set with Docker-Compose](https://zgadzaj.com/development/docker/docker-compose/turning-standalone-mongodb-server-into-a-replica-set-with-docker-compose)
- [Creating a MongoDB replica set using Docker 🍃](https://www.sohamkamani.com/blog/2016/06/30/docker-mongo-replica-set/)
- [
asoorm/docker-compose-mongo-replicaset.yml](https://gist.github.com/asoorm/7822cc742831639c93affd734e97ce4f)
