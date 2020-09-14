


based on:
docker pull bitnami/spark:latest

create a network:
docker network create spark-docker-network

spark-master,
spark-worker:
docker container run -it -d --name spark-docker-master \
--network spark-docker-network \
-p 8080:8080 \
-p 4040:4040 \
-e SPARK_MODE=master \
bitnami/spark:latest

docker container run -it -d --name spark-docker-worker-1 \
--network spark-docker-network \
-p 4041:4040 \
-p 8081:8081 \
-e SPARK_MODE=worker \
-e SPARK_MASTER_URL=spark://spark-docker-master:7077 \
-e SPARK_WORKER_MEMORY=2G \
-e SPARK_WORKER_CORES=2 \
bitnami/spark:latest

docker container run -it -d --name spark-docker-worker-2 \
--network spark-docker-network \
-p 4042:4040 \
-p 8082:8081 \
-e SPARK_MODE=worker \
-e SPARK_MASTER_URL=spark://spark-docker-master:7077 \
-e SPARK_WORKER_MEMORY=2G \
-e SPARK_WORKER_CORES=2 \
bitnami/spark:latest

spark-shell --master spark://spark-docker-master:7077 --executor-memory 1G --executor-cores 1 --num-executors 3
