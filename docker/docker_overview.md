# Docker Overview
 > Docker is an open platform for developing, shipping, running applications
 
 각각의 기능들에 대해서 이야기 해 보자.
 - developing 
   - docker를 활용해서 Laptop PC에서 작업하던 것을 그대로 Macbook으로 가져와 동일한 환경에서 작업할 수 있다. 
   - 마찬가지로 Macbook에서 작업하던 것을 그대로 windows 환경에서 개발할 수 있다. 
   - 어떠한 OS든 동일한 환경에서 개발을 계속 할 수 있다. 
 - shiping
   - 이미지를 저장소에 올려 놓으면 쉽게 가져다 동작 시킬 수 있다. 
 - running application
   - Docker로 빌드된 어떠한 이미지도 docker daemon이 실헹된 곳에서 동작한다. 

- Docker는 당신의 infrastructure가 뭐든 상관없이 빠르게 동작 가능하도록 디자인 되었다. 
- Docker를 통해서..
   - code faster
   - test faster
   - deploy faster
   - shorten the cycle between writing code and running code.
  
- docker image로 제작하면 특별한 문서 없어도 applicaton 전달이 쉽다. 

## What is the Docker platform?
- 어떠한 application이든 안전하고 독립된 container 형태로 서비스 가능하게 한다. 
- 안전하고 독립된 형태의 container라는 것이 매우 중요한 개념이다. 가령, 하나의 물리 장비에서 여러개의 process를 동작시키면 memory, cpu 등 자원을 공유 하게 되고 어떻게든 프로세스간 간섭이 생기기 마련이다.
- container를 활용하면 동일 물리 장비에서 마치 독립된 host에서 동작하는 것과 같이 자원을 분리해서 사용가능하다. 
- docker container형태로 전달하고 배포할 수 있다. 
- 개인별 독립된 production 환경뿐 아니라 local data center 및 cloud 환경으로도 배포 가능하다.




 
