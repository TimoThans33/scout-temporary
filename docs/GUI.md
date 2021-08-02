# 5. Scout GUI

Documentation on the scout-gui

## 5.1 Installation
The GUI software is installed manually.

### 5.1.1 Install Docker
We heavily rely on docker for containarization. The latest version should be installed on the scout. Refers to https://docs.docker.com/engine/install/ubuntu/
- install using apt package manager
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli container.io
```

> in case apt does not find the packages update the apt repository. This is shown in link above.

- You should be able to run docker as user.
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
- Log out and back in so that your group membership is re-evaluated.
```
newgrp docker
```
- Please verify that docker is installed correctly with
```
docker run hello-world
```
### 5.1.2 Install MongoDB
We use mongoDB to store the generated path. Refers to https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/.
- add the apt key
```
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
```
- add to apt list
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
```
- update package manager and install with apt.
```
sudo apt update
sudo apt-get install -y mongodb-org
```

### 5.1.3 Install GUI
- extract scout-gui.zip using file-explorer or terminal
```
pvadmin@<scout_ip>:/home/pvadmin/envs/cx-appcenter/apps/
```
- copy the service file
```
sudo cp /home/pvadmin/envs/cx-appcenter/apps/scout-gui/0.1-unstable/cx_scout-gui_0.1-unstable@.service  /etc/systemd/system/
```
- build the container
```
/home/pvadmin/envs/cx-appcenter/apps/scout-gui/0.1-unstable/cx-auto-mapping-service/./build.sh
```
- edit the ip address in the settings to the scout ip-address
```
nano /home/pvadmin/envs/cx-appcenter/apps/scout-gui/0.1-unstable/cx-auto-mapping-service/settings.env
```
- restart the daemon and start systemd service
```
sudo systemctl daemon-reload
sudo systemctl enable cx_scout-gui_0.1-unstable@1.service
sudo systemctl start cx_scout-gui_0.1-unstable@1.service
```
---
## 5.2 Usage