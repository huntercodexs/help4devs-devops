
# DOCKER HELPER


### Ubuntu 20.04

- Docker Installing

<pre>
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
</pre>

- Docker Uninstall

<pre>
sudo apt-get remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
</pre>

### Almalinux 8.10

- Docker Installing

<pre>
sudo dnf --refresh update
sudo dnf upgrade
sudo dnf install yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin
</pre>

- Docker Uninstall

<pre>
sudo dnf remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
</pre>

### Docker Management

- Give access to current $USER into docker group to avoid the use of sudo command ever

<pre>
sudo usermod -aG docker ${USER}
su - ${USER}
</pre>

- Managing Docker services

<pre>
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo systemctl stop docker
sudo systemctl start docker
sudo systemctl status docker
</pre>

- Docker commands useful

<pre>
docker ps
docker run
docker images ls
docker image ls
docker container ls
docker volumes ls
docker system prune -a
docker rmi -v ${IMAGE_ID}
docker network create ${NETWORK_NAME}
docker exec -it ${CONTAINER_NAME} [${COMMAND}[OPTIONS]]
[Example]
docker exec -it mailhog-ubuntu2004 /bin/bash
</pre>

### Docker Compose Installing

- Installing the current release of docker-comopse

<pre>
sudo curl -L "https://github.com/docker/compose/releases/download/${VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
</pre>

- Make docker-compose executable

<pre>
sudo chmod +x /usr/local/bin/docker-compose
</pre>

- Check the Docker Compose Version

<pre>
docker-compose --version
</pre>

### Clear and reset the host machine of docker containers

> IMPORTANT: It will be removed all containers, images and configurations from the docker

<pre>
docker network rm ${NETWORK_NAME}
docker-compose down --volumes
docker system prune -a
    WARNING! This will remove:
      - all stopped containers
      - all networks not used by at least one container
      - all images without at least one container associated to them
      - all build cache
    
    Are you sure you want to continue? [y/N] y
</pre>

### Some helpful docker compose commands

<pre>
docker-compose ps
docker-compose --build
docker-compose up --build
docker-compose up -d
docker-compose up
docker-compose start
docker-compose stop
docker-compose down [--volumes]
docker-compose logs
docker-compose pause
docker-compose unpause
</pre>

### Example: Docker + Docker Compose - Using Private Subnet

> One simple example of this environment can be seemed in the GitHub repository by the following link 
> https://github.com/huntercodexs/docker-series/tree/nginx_reverse_proxy_python_mysql_separated

### Example: Docker + Docker Compose - Using Public Subnet

> Please use the repository "ELK PROMETHEUS GRAFANA ZIPKIN MYSQL" hosted in the GitHUB.
> Just click on the link https://github.com/huntercodexs/docker-series/tree/elk_prometheus_grafana_zipkin_mysql_v1

