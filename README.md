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

### Run "Reddit-app via docker-compose" with Prometheus monitoring
- Prereq: installed on local machine docker-compose
```
export USER_NAME=<username> // your DockerHub login
```

- Build Prometheus image
```
cd prometheus
docker build -t $USER_NAME/prometheus .

```
- Build microservices images:
```
/ui (microservices ✔) $ bash docker_build.sh
/post-py (microservices ✔) $ bash docker_build.sh
/comment (microservices ✔) $ bash docker_build.sh
```

- Use example.env to make your own copy of ```.env``` file with variables
```
cd reddit-app/
docker-compose up -d  #run the app
docker-compose ps #check app is running
```

- App will be running on :9292 port
- Prometheus will be running on 9090 port

### Run "Reddit-app in docker-swarm cluster

- Build microservices images
- Run instances <master-1><worker-1><worker-2>:
```
docker-machine create --driver google \
   --google-project  dockerswarm-188616  \
   --google-zone europe-west1-b \
   --google-machine-type g1-small \
   --google-machine-image $(gcloud compute images list --filter ubuntu-1604-lts --uri) \
   <instance name>
```
- On master node init cluster:
```
docker swarm init
```
- Join worker-nodes to cluster:
```
docker swarm join --token <token> <advertise manager address>:2377
```
- To check cluster state - run on Master:
```
docker node ls
```
- Deploy services to cluster
```
docker stack deploy --compose-file=<(docker-compose -f docker-compose.yml config 2>/dev/null) DEV
```
- Check services states
```
docker stack services DEV
```
