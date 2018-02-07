# SQL Server in Docker a new defaince Linux

## Instalación de Ubuntu

#### Instalar ubuntu 16.04 de forma estandar.
#### crear usuario _mssql_.

## Instalación de Docker
#### Remover vesiones antiguas.
 
```
sudo apt remove docker docker-engine docker.io
```

#### instalar dependencias.

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

#### instalar repositorio.
  - Agregar llave

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

  - Agregar fingerprint.

```
sudo apt-key fingerprint 0EBFCD88
```

  - Agregar Repositorio.

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

  - Actualizar repositorio.

```
sudo apt update
```

#### Instalar _docker-ce_.

```
sudo apt install docker-ce
```

#### Instalar _docker-compose_.

```
sudo apt install docker-compose
```

## Instalación y configuración de mssql
#### Agregar grupo _docker_ a usuario de _mssql_. Después se debe ingresar o reingresar al sistema con usuario _mssql_.

```
sudo usermod -aG docker mssql
```

#### Con el usuario _mssql_, crear volumen _mssql-db_, para los respaldos de la base de datos. La ruta por omisión es _"/var/lib/docker/volumes/mssql-db"_.

```
docker volume create --name=mssql-db
```

#### Crear archivo _docker-compose.yml_.

```

version: '2'

services:

  mssql:
    image: microsoft/mssql-server-linux:latest
    container_name: mssql
    restart: always
    networks:
      - mssql-tier
    ports:
      - 1433:1433
    expose:
      - 1433
    volumes:
      - mssql-db:/var/opt/mssql
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=P4ssw0rd+
      - MSSQL_PID=Developer

volumes:
  mssql-db:
    external: true

networks:
  mssql-tier:
    driver: bridge
```

#### Dejar como servicio a docker.

```
sudo systemctl enable docker
sudo systemctl start docker
```

#### Comprobar que archivo _docker-compose.yml_, está editado de forma correcta. Una vez esté corriendo el servicio, se puede probar con _"SQL Operation Studio"_.

```
docker-compose up
```

#### Detener ejecución del contenedor _mssql_, para ejecutar en segundo plano.

```
docker-compose stop
docker-compose up -d
```

#### Verificar que está corriendo el contenedor _mssql_.

```
docker ps
```

#### Vereficar que no está corriendo el contenedor _mssql_.

```
docker ps -a
```
