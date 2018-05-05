#### 使用Docker Swarm

##### 创建镜像
```bash
git clone https:github.com/leipengkai/docker-drf.git
cd Dockerfiles
docker-compose up
# 再开另一個 terminal，先使用 docker ps 找到正在执行的 web container
docker commit -m "create" CONTAINER_ID leipengkai/drf
docker login
docker push leipengkai/drf
# 使用 docker stack 的方式部署,而 docker stack 強制规定一定要使用 image

```

##### 创建主机
```bash
docker-machine create -d virtualbox vm1
docker-machine create -d virtualbox vm2
docker-machine create -d virtualbox vm3

# 连接vm1
docker-machine ssh vm1
docker swarm init --advertise-addr 192.168.99.100(vm1:ip)
# 上面的命令会提示vm1是管理员,并生成如下命令
# 只需要连接vm2,vm3运行如下命令就可以成为node
docker swarm join --token SWMTKN-1-5b08c43sjxzeg2gk4xjcmfkrn41ap7i77okitjr7a45ja1czd5-0td3csojve9nsbwja1f0vzn4b 192.168.99.100:2377

```

#### 开始部署
```bash
docker-machine ssh vm1
git clone https:github.com/leipengkai/docker-swarm.git

cd docker-swarm
# 部署
docker stack deploy --compose-file docker-stack.yml my_app

docker service ls
docker ps
docker exec -it < web Container ID> bash
	web> python3 manage.py makemigrations musics
	web> python3 manage.py migrate
	web> python3 manage.py createsuperuser
	web> python3 manage.py collectstatic
# 访问
192.168.99.100:8000/api/v1/
192.168.99.100:8080
```


