# microservices

Build "Reddit" image:

```
docker build -t reddit:latest .
```

Run application:

```
docker run --name reddit -d --network=host reddit:latest
```

Docker hub image link:

https://hub.docker.com/r/ksevasilyeva/otus-reddit/