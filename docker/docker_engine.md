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
``docker COMMAND```가 Docker Client 동작이며, ```docker daemon```이 Docker Daemon 즉 서버역할을 한다. 

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
- gorilla/mux에서 제공하는 mux.Router는 HTTP 요청 Method단위로 URL를 지정해 놓으면 해당 요청에 할당된 handler를 호출하는 구조로 Docker RESTful API를 처리하기 최적의 구조이다.
```
https://github.com/docker/docker/tree/master/api/server/server.go

func createRouter(s *Server) *mux.Router {
 ...
 ...
 m := map[string]map[string]HttpApiFunc {
   "GET": {
       "/_ping":                  s.ping,
       "/events":                 s.getEvents,
       "/info":                   s.getInfo,
       "/images/json":            s.getImageJSON,
       ...
       ...
       ...
   },
   "POST": {
      "/auth":                    s.postAuth,
      ...
      "/images/create":           s.postBuild,
      ...
      
      
      ...
      "/exec/{name:.*}/start":    s.postContainerExecStart,
   },
   "DELETE": {
     "/containers/{name:.*}":     s.deleteContainers,
     ... 
   },
   "OPTIONS": {
      "":                         s.optionsHandler,
   },
}

```
- http server에서 Client Docker로 부터 요청이 들어오면 새로운 goroutine를 생성하고 요청 content를 분석하여 routing해 준다. 처리가 가능한 handler를 배정하고 해당 handler이 요청을 처리하게 된다.
- handle은 요청을 처리하기 위해 engine이 제공하는 여러가지 형태의 job를 활용하게 된다.

### Docker Engine
- Docker의 Core 모듈이 모여있다. 
   - Docker storage
   - management container
   - management register
   - management driver
- Engine은 여러개의 job들로 구성되며 job은 특정 동작을 실행하는 process로 보면 된다. 
 - create a new container
 - HTTP API processing 
 - Reigster info procssing


    
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

