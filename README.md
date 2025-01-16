# References #

- https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04
- https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
- https://docs.docker.com/engine/reference/commandline/cli/
- https://hub.docker.com/search?q=

# Docker Setup #

```
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker

sudo usermod -aG docker ${USER}

su - ${USER}

groups

sudo usermod -aG docker username
# Note replace username with your actual username.
```

# Setting up NVIDIA Container Toolkit #

```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update

sudo apt-get install -y nvidia-container-toolkit

sudo nvidia-ctk runtime configure --runtime=docker

sudo systemctl restart docker
```

# Docker Commands #

**To see the images that have been downloaded to your computer**
```
docker images
```

**To view the active containers**
```
docker ps
```

**To view all containers**
```
docker ps -a
```
**To start a container**
```
docker start name_of_the_container
```

**To stop a container**
```
docker stop name_of_the_container
```

**To remove a container or image**
```
docker rmi name_of_the_image
docker rm name_of_the_container
```

**Remove build cache**
```
docker builder prune
```

**Remove unused data**
```
docker system prune
```
# Issues #

* Cuda error - restart the container if Cuda is NOT available.


# Dockerfile #

- Create a Dockerfile

- Bulding a docker image when having a docker file

```
docker build -t name_of_the_image -f Dockerfile .
```

# Create and login to the docker container
```
docker run --gpus device=all \
    -v ~/workspace/my_project:/root/workspace/my_project \
    -v ~/workspace/share:/root/workspace/share \
    -v ~/workspace/my_project/storage/docker_cache:/root/.cache \
    --network host \
    --init -it --restart unless-stopped \
    --name name_of_the_image \
    name_of_the_container \
    bash
```

**Login to the container**
```
docker exec -it name_of_the_container bash
```