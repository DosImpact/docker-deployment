# redis Service

## redis cache service


## redis Q Service

- run

docker run -d \
  -e REDIS_PASSWORD=dosimpact\
  -p 6000:6379 \
  --name redis_q \
  --restart always \
  redis:latest /bin/sh -c 'redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}'

- cli
redis-cli -h 221.153.254.18 -p 16000 -a dosimpact
