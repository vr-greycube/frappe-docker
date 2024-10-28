```
sudo apt update && sudo apt upgrade && sudo apt autoremove

sudo apt install git curl
```

#### Install Docker

https://docs.docker.com/engine/install/ubuntu/

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
Add Docker's official GPG key:
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update	

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Dcoker sudo
https://docs.docker.com/engine/install/linux-postinstall/
```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

#### Install Docker Compose
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04

use the latest release url in command below

```
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.x.x/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```

#### SSH
```
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
# Copy SSH pub key to github https://github.com/settings/keys
```


#### Install VSCode
https://code.visualstudio.com/docs/setup/linux

Download .deb and 
```
sudo apt install ./file.deb
```

Docker references
https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/





#### Frappe 
- docker compose adapted from https://github.com/Avunu/frappe-bench-docker
- https://github.com/frappe/frappe_docker for a more complete setup



```
git clone https://github.com/vr-greycube/frappe-docker
cd frappe-docker/version-15
sudo docker compose up

```

#### To access the bench inside the container from host
```
# copy id of frappe backend container to access the bench
sudo docker ps | grep backend

sudo docker exec -it <container-id> /bin/bash

```

#### To install apps
```
bench get-app erpnext --branch version-15
```


#### To create a site

```
# once inside container, use bench as usual

# IMPORTANT: use --no-mariadb-socket. 
# Else the db user is created as xyz@a.b.c.d and will not be able to connect as the container IP changes (on restart)

bench new-site demo15.localhost --db-name demo15 --no-mariadb-socket
```

#### To create a new Bench

- Create a folder for the new bench and copy the files (excluding frappe-bench dir) from version-15 or version-14
- Changes in .env file
  - FRAPPE_VERSION
  - PYENV_VERSION
  - MARIADB_PORT
  - WEB_PORT
  - Note: sites created in this bench will be accessible at site_name.localhost:WEB_PORT on the host


#### bench helper
bench.sh is a script to launch a shell inside the container in the bench folder
```
sudo chmod +x ./bench.sh
./bench.sh
```

#### DBeaver

Install [DBeaver](https://dbeaver.io/download/) for quick db edits & writing SQL. 



- If a new site was created without the --no-mariadb-socket flag, can try to create user directly in db with DBeaver
```
use myql;
SELECT * from user;
CREATE USER 'demo15'@'%' IDENTIFIED BY 'site_config-password';
RENAME USER 'DEMO15'@'%' TO 'demo15'@'%' IDENTIFIED BY 'site_config-password';
GRANT ALL PRIVILEGES ON demo15.* To 'demo15'@'%' IDENTIFIED BY 'site_config-password';
```


To stop and remove all containers
```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

#### Debug with VSCode



#### Jupyter
