

docker network create --subnet=172.20.0.0/16 testip1


docker create --net testip1 --ip 172.20.0.3 --name cont3 ubuntu /bin/bash -c "sleep 5000"
docker container start cont3

docker create --net testip1 --ip 172.20.0.4 --name cont4 ubuntu /bin/bash -c "sleep 5000"
docker container start cont4

docker create --net testip1 --ip 172.20.0.5 --name cont5 ubuntu /bin/bash -c "sleep 5000"
docker container start cont5



docker network inspect testip1

docker create --net testip1 --name contX ubuntu /bin/bash -c "sleep 5000"
docker container start contX
docker create --net testip1 --name contY ubuntu /bin/bash -c "sleep 5000"
docker container start contY
docker create --net testip1 --name contZ ubuntu /bin/bash -c "sleep 5000"
docker container start contZ


docker inspect network testip1

docker inspect container cont3
