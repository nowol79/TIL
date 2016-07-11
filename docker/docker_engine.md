# Docker Total Architecture
![docker_total_architecture](https://cloud.githubusercontent.com/assets/9585881/16713210/eab33898-46da-11e6-9052-7a8679c44478.PNG)

# Docker Engine 구성
![docker_engine](https://cloud.githubusercontent.com/assets/9585881/16617978/d14d0058-43c1-11e6-8acb-5c694916a951.PNG)

- client-server 구성
- server : daemon process
- REAT API : daemon으로 명령을 보낼 수 있는 interface
- CLI(command line interface) client : REST API를 command line에서 명령어 형태로 사용할 수 있게 wrapping한 것이다. 

# Docker 
docker 실행하면, 하나의 실행파일이 두 가지 버젼으로 동작한다. 
```
$ docker
Usage: docker [OPTIONS] COMMAND [arg...]
       docker daemon [ --help | ... ]
       docker [ --help | -v | --version ]
```
docker COMMAND가 Docker Client 동작이며, docker daemon이 데몬 즉 서버역할을 한다. 

## Docker Client
- Docker daemon하고 통신한다.
  - tcp://host:port or unix://path_to_socket 으로 connection를 맺는다. 
   - 이때 --tlsverify 옵션이 켜져 있으면 인증서 교환하고 TLS 통신
  - docker daemon과 connection이 성공하면, docker RESTful API를 던져 원하는 정보를 얻는다.
   ```
   connect(3, {sa_family=AF_FILE, path="/var/run/docker.sock"}, 23) = 0
   write(3, "GET /v1.23/containers/json HTTP/"..., 89) = 89
   ```

## Docker daemon(server)
- background 형태의 데몬으로 Docker Client 요청을 처리한다. 
![docker-client](https://cloud.githubusercontent.com/assets/9585881/16717856/fef45c08-4755-11e6-837c-838024d24035.PNG)

- Client 요청을 처리하기 위한 Docker http Server가 있다.
- 사용자 요청별 job를 할당해서 처리를 한다. 
### Docker Server
Client 요청을 처리하기 위한 Http.Server, 요청을 스케쥴링하기 위한 Router, 실제 처리를 담당하는 Handler로 구성된다.
![docker-engine](https://cloud.githubusercontent.com/assets/9585881/16718085/3dca2100-4757-11e6-8358-7f2d4251e432.PNG)

  - Client 요청을 처리
 - caps
 - cluster
 - events
 - exec
 - graphdriver
 - links
 - logger
 - network
 
### api
 - client - Docker CLI
  - pull, run, build,... 
 - server
 



## manages object
### image
### container
### network
### data volumes
### registry

## utils
### oci
### plugin

