# SQL Server in Docker a new defaince Linux
Tested on Ubuntu 16.04. 

## Ubuntu install

#### Standard installation of Ubuntu 16.04.
#### Create group and user _mssql_.
Create group, user and change password.
```
sudo groupadd mssql
sudo useradd -d /home/mssql -m -s /bin/bash -g mssql -G mssql,adm,sudo mssql
sudo passwd mssql
```

## Docker install
#### Remove older version.
 
```
sudo apt remove docker docker-engine docker.io
```

#### Dependencies install.

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common git
```

#### Add Repository
  - Add key

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

  - Add fingerprint.

```
sudo apt-key fingerprint 0EBFCD88
```

  - Add Repository.

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

  - Update Repository.

```
sudo apt update
```

#### Intatall _docker-ce_.

```
sudo apt install docker-ce
```

#### Install _docker-compose_.

```
sudo apt install docker-compose
```

## Installation and configuration
#### Add group _docker_ to user _mssql_.

```
sudo usermod -aG docker mssql
```
login or re-login with _mssql_.

#### Create volume _vol-mssql_.
```
mkdir -p /home/mssql/mssql-vol/
docker volume create --opt type=none --opt device=/home/mssql/mssql-vol --opt o=bind --name=mssql-vol
```
or
```
docker volume create --name=mssql-vol
```
The default path is _"/var/lib/docker/volumes/mssql-vol"_.

#### Activate docker as a service.

```
sudo systemctl enable docker
sudo systemctl start docker
```

#### Clone https://github.com/cfranciscoj/linux-docker-mssql.git.
```
git clone https://github.com/cfranciscoj/linux-docker-mssql.git
```
Enter to linux-docker-mssql.
```
cd linux-docker-mssql
```

#### Run docker-compose.

For seeing does SQL Server work, try this and check with your SQL Operation Studio.
```
docker-compose up
```

#### Stop _mssqlcont_ container and start "SQL Server" in background.

```
docker-compose stop 
docker-compose up -d
```

#### Check that the _mssqlcont_ container is running.

```
docker ps
```

#### Check that the _mssqlcont_ container is stopped.

```
docker ps -a
```

#### Stop _mssqlcont_ container.

```
docker stop mssqlcont
```

#### Start _mssqlcont_ container.

```
docker start mssqlcont
```

## Uninstall.
#### Stop _mssqlcont_ container.

```
docker stop mssqlcont
```
#### Remove _mssqlcont_ container.

```
docker rm mssqlcont
```
#### Remove _vol-mssql_ volume.

```
docker volume rm vol-mssql
```

#### Remove _vol-mssql_ folder.

```
sudo rm -Rf /home/mssql/vol-mssql
```

