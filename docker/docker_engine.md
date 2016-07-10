# Docker Total Architecture
![docker_total_architecture](https://cloud.githubusercontent.com/assets/9585881/16713210/eab33898-46da-11e6-9052-7a8679c44478.PNG)

# Docker Engine 구성
![docker_engine](https://cloud.githubusercontent.com/assets/9585881/16617978/d14d0058-43c1-11e6-8acb-5c694916a951.PNG)

- client-server 구성
- server : daemon process
- REAT API : daemon으로 명령을 보낼 수 있는 interface
- CLI(command line interface) client : REST API를 command line에서 명령어 형태로 사용할 수 있게 wrapping한 것이다. 

## Core
### daemon(server)
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
 
### cli


## manages object
### image
### container
### network
### data volumes
### registry

## utils
### oci
### plugin


참고 : http://prog3.com/sbdm/blog/shlazww/article/details/39188933
