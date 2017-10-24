# microservices
### Run Monolith Reddit app

- Build "Reddit" image:

```
cd monolith/
docker build -t reddit:latest .
```

- Run application:

```
docker run --name reddit -d --network=host reddit:latest
```

- Docker hub image link:

https://hub.docker.com/r/ksevasilyeva/otus-reddit/

### Run "Reddit-app"

- Build components images:
```
cd reddit-app/
docker build -t ksevasilyeva/post:1.0 ./post-py
docker build -t ksevasilyeva/comment:1.0 ./comment
docker build -t ksevasilyeva/ui:1.0 ./ui
```
- Create common network `docker network create reddit`
- Create DB data volume  `docker volume create reddit_db`

- Run application:

```
docker run -d --network=reddit -v reddit_db:/data/db --network-alias=post_mongo --network-alias=comment_mongo mongo:latest
docker run -d --network=reddit --network-alias=post ksevasilyeva/post:1.0
docker run -d --network=reddit --network-alias=comment-ui ksevasilyeva/comment:1.0
docker run -d --network=reddit -p 9292:9292 ksevasilyeva/ui:1.0
```