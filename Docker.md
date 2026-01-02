Docker = a way to run software in isolated, lightweight environments called containers

Think of Docker as:

📦 “Run any software, with all its dependencies, without installing it on your machine”


Commands
- docker ps
- docker-compose up -d
- docker-compose down
- docker system prune -f
- docker exec -it kafka \
kafka-topics --create \
--topic demo-topic \
--bootstrap-server localhost:9092 \
--partitions 1 \
--replication-factor 1

