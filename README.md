Docker images to run RabbitMQ cluster. It extends the official image with a rabbitmq-cluster script that does the magic.

# Building

Use the following command to build the images locally. This is not required if you are willing to use the docker hub image which is already uploaded.

```
docker build -t sanjaysingh/rabbitmq-cluster .
```

# Running with docker-compose

```
docker-compose up -d
```

By default 3 nodes are started up this way:

```
rabbit1:
      image: sanjaysingh/rabbitmq-cluster
      hostname: rabbit1
      environment:
        - ERLANG_COOKIE=abcdefg
      ports:
        - "5691:5672"
        - "15691:15672"
    rabbit2:
      image: sanjaysingh/rabbitmq-cluster
      hostname: rabbit2
      depends_on:
        - "rabbit1"
      environment:
        - ERLANG_COOKIE=abcdefg
        - CLUSTER_WITH=rabbit1
        - ENABLE_RAM=true
      ports:
        - "5692:5672"
        - "15692:15672"
    rabbit3:
      image: sanjaysingh/rabbitmq-cluster
      hostname: rabbit3
      depends_on:
        - "rabbit1"
      environment:
        - ERLANG_COOKIE=abcdefg
        - CLUSTER_WITH=rabbit1
      ports:
        - "5693:5672"
        - "15693:15672"
```


Once cluster is up:
* The management console can be accessed at `http://localhost:15691`, `http://localhost:15692` and `http://localhost:15693`


# Credits

* Inspired by https://github.com/harbur/docker-rabbitmq-cluster


