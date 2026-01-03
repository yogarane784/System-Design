Docker = a way to run software in isolated, lightweight environments called containers

Think of Docker as:

📦 “Run any software, with all its dependencies, without installing it on your machine”


Commands
- docker ps
- docker ps -a // includes stopped containers
- docker-compose up -d
- docker-compose down
- docker stop container_id
- docker rm container_id
- docker system prune -f
- docker images
- docker logs container_id
- docker logs -f container_id // -f is to follow logs (like tail -f)
- docker pull redis // Download image from Docker Hub / registry
- docker pull openjdk:17 // Download image from Docker Hub / registry
- docker build -t my-app . // Build an image from Dockerfile
- docker exec -it kafka \
kafka-topics --create \
--topic demo-topic \
--bootstrap-server localhost:9092 \
--partitions 1 \
--replication-factor 1

