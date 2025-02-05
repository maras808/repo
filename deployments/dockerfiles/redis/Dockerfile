FROM redis:8.0-M03-alpine3.21
COPY apps/redis/redis.prod.conf /usr/local/etc/redis/redis.conf
CMD redis-server /usr/local/etc/redis/redis.conf