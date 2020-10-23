-v /data/redis/conf/redis.conf:/usr/local/etc/redis/redis.conf 

docker run -d -p 6379:6379  -v /data/redis:/data -v ./redis.conf:/data/redis/conf/redis.conf -c 'redis.conf /usr/local/etc/redis/redis.conf' --name redis redis redis-server --requirepass redis123 --appendonly yes --aclfile yes


docker run -v /data/redis/conf/redis.conf:/usr/local/etc/redis/redis.conf --name myredis redis -p6379:6379 redis-server /usr/local/etc/redis/redis.conf

docker run -ti -p 6379:6379  -v /data/redis:/data --name redis redis aclfile /data/redis/conf/users.acl redis-server --requirepass redis123 --appendonly yes

docker run -ti -p 6379:6379  -v /data/redis:/data --name redis redis redis-server --requirepass redis123 --appendonly yes --aclfile /data/redis/conf/users.acl
