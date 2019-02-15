**redis cluster安装**

一、安装redis

    cd /usr/local/
    wget http://download.redis.io/releases/redis-3.0.6.tar.gz
    tar -zxvf redis-3.0.6.tar.gz 
    cd redis-3.0.6 redis
    make && make install
    mkdir /usr/local/redis_cluster  //创建集群目录
    cd /usr/local/redis_cluster/
    mkdir 7000 7001 7002 7003 7004 7005  // 其对应端口

    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7000  
    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7001  
    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7002
    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7003
    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7004
    cp /usr/local/redis-3.0.6/redis.conf  /usr/local/redis_cluster/7005


二、修改配置文件redis.conf配置文件，启动
   
    redis-3.0.6/src/redis-server redis_cluster/7000/redis.conf
    redis-3.0.6/src/redis-server redis_cluster/7001/redis.conf
    redis-3.0.6/src/redis-server redis_cluster/7002/redis.conf
    redis-3.0.6/src/redis-server redis_cluster/7003/redis.conf
    redis-3.0.6/src/redis-server redis_cluster/7004/redis.conf
    redis-3.0.6/src/redis-server redis_cluster/7005/redis.conf

三、创建集群

  前面已经准备好了搭建集群的redis节点，接下来我们要把这些节点都串连起来搭建集群。官方提供了一个工具：redis-trib.rb(/usr/local/redis-3.2.1/src/redis-trib.rb) 看后缀就知道这鸟东西不能直接执行，它是用ruby写的一个程序，所以我们还得安装ruby.

    yum -y install ruby ruby-devel rubygems rpm-build 
  再用 gem 这个命令来安装 redis接口    gem是ruby的一个工具包.

    gem install redis    //等一会儿就好了
  当然，方便操作，两台Server都要安装。上面的步骤完事了，接下来运行一下redis-trib.rb

    如果报如下错误
      注意：redis requires Ruby version >= 2.2.2
      解决方案：https://blog.csdn.net/fengye_yulu/article/details/77628094



  看到这，应该明白了吧， 就是靠上面这些操作 完成redis集群搭建的.
  确认所有的节点都启动，接下来使用参数create 创建 (在39.104.80.206中来创建)
    
    #./redis-trib.rb create --replicas 1 XXXX:PORT1 XXXX:PORT2 ....
    /usr/local/redis-3.0.6/src/redis-trib.rb  create  --replicas  1  39.104.80.206:7000 39.104.80.206:7001  39.104.80.206:7002 39.104.80.206:7003  39.104.80.206:7004  39.104.80.206:7005
    

    创建的时候一直等待 Waiting for the cluster to join 很久都没有反应
    原因：
        redis集群不仅需要开通redis客户端连接的端口，而且需要开通集群总线端口
        集群总线端口为redis客户端连接的端口 + 10000
        如redis端口为6379
        则集群总线端口为16379
        故，所有服务器的点需要开通redis的客户端连接端口和集群总线端口
        注意：iptables 放开，如果有安全组，也要放开这两个端口
              阿里云内网访问不需要开放端口。



