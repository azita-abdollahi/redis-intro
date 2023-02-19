# What is Redis?

[Redis](https://en.wikipedia.org/wiki/Redis) is an open-source, networked, in-memory, key-value data store with optional durability. It is written in ANSI C.The name Redis means REmote DIctionary Server.

## How to run and deploy Redis in Docker?

### 	pull image

```bash
docker pull redis
```

### 	run container

```
docker run -d --rm --name redis -p 6379:6379 redis
```

​	If you don't have the image, this command will pull it. And then, if you need to access from **redis-cli** to console, can 	use:

```
docker exec -it redis redis-cli
```
