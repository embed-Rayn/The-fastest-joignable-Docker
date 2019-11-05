# The-fastest-joignable-Docker: 도커 공부를 위한 저장소
본 문서는 도커를 공부하는 분들을 대상으로 작성되었습니다.
추가되었으면 좋겠다 생각하시는 자료를 알려주시면 반영하도록 하겠습니다.

_본 문서는 아래와 같은 규칙을 따라 작성되었습니다._
- [가장 빨리 만나는 Docker](http://pyrasis.com/docker.html)를 바탕으로 정리합니다.
- 저의 개념에 따라 목차가 다소 변경될 수 있습니다.
- 현업 적용을 위한 컨테이너, 이미지, 도커파일 등 간략한 명령어 위주로 정리했습니다.
<br/>

## 2.도커 설치하기
[링크 대체](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter02)
<br/>
## 3.도커 명령
Docker의 명령은 docker <명령><br/>
항상 root 권한(sudo)으로 실행
### 이미지 관련 명령어
| Command | Description | Format<br/>example |
|:---:|:---|:---|
| images | 보유한 이미지의 목록 출력 | sudo docker images |
| search | Docker Hub에서 이미지를 검색 | sudo docker search <검색할 이미지><br/>sudo docker search ubuntu |
| pull | Docker Hub에서 이미지를 다운로드 | sudo docker pull <이미지 이름>:<태그><br/>sudo docker pull ubuntu:latest |
| run | 이미지를 컨테이너로 생성한 뒤 Bash 셸을 실행 | sudo docker run <옵션> <이미지 이름> <실행할 파일><br/>sudo docker run -it --name hello ubuntu /bin/bash |
| rmi | 이미지 삭제 | sudo docker rmi <이미지 이름>:<태그><br/>sudo docker rmi ubuntu:latest |

- **<이미지 이름> 대신 이미지 ID를 사용해도 됩니다.** <br/>
- **태그를 생략할 경우 pull은 latest 태그만 다운로드, rmi는 모든 태그를 삭제합니다.** <br/>

#### docker run 옵션
옵션	설명
| Option | Description |
|:---:|:---|
| -d | detached mode 흔히 말하는 백그라운드 모드 |
| -p | 호스트와 컨테이너의 포트를 연결 (포워딩) |
| -v | 호스트와 컨테이너의 디렉토리를 연결 (마운트) |
| -e | 컨테이너 내에서 사용할 환경변수 설정 |
| –name | 컨테이너 이름 설정 |
| –rm | 프로세스 종료시 컨테이너 자동 제거 |
| -it | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| –link | 컨테이너 연결 [컨테이너명:별칭] |

### 컨테이너 관련 명령어
| Command | Description | Format<br/>example |
|:---:|:---|:---|
| ps | 모든 컨테이너 목록을 출력<br/>-a 옵션 이용시 종료돈 컨테이너 목록도 출력 | sudo docker ps <옵션><br/>sudo docker ps -a |
| start | 정지된 컨테이너 시작 | sudo docker start <컨테이너 이름><br/>sudo docker start hello |
| restart | 컨테이너 재시작 | sudo docker restart <컨테이너 이름><br/>sudo docker restart hello |
| attach | 백그라운드에서 시작한 컨테이너 접속 | sudo docker attach <컨테이너 이름><br/>sudo docker attach hello |
| exec | 백그라운드에서 실행된 컨테이너의 명령 실행 | sudo docker exec <컨테이너 이름> <명령> <매개 변수><br/>sudo docker exec hello echo "Hello World" |
| stop | 컨테이너 정지 | sudo docker stop <컨테이너 이름><br/>sudo docker stop hello |
| rm | 컨테이너 삭제 | sudo docker rm <컨테이너 이름><br/>sudo docker rm hello |

- *<컨테이너 이름> 대신 컨테이너 ID를 사용해도 됩니다.* <br/>

### 컨테이너 Bash 셸 내부 명령어
| Command | Description |
|:---:|:---|
| exit or ctrl + d | 컨테이너를 stop 후 빠져나옴 |
| ctrl + q, ctrl + p | 컨테이너를 정지하지 않고 Bash 셸 나옴 |
<br/>

## 4.Docker 이미지 생성하기
### Dockerfile 생성하기
__예제 코드__ 한 폴더에 ./Dockerfile을 만들어 줍니다.
```
FROM ubuntu:14.04
MAINTAINER Foo Bar <foo@bar.com>

USER root
WORKDIR /tmp
RUN apt-get update
RUN apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx

VOLUME ["/data", "/etc/nginx/site-enabled", "/var/log/nginx"]

WORKDIR /etc/nginx

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
```
| Command | Description |
|:---:|:---|
| FROM | 어떤 이미지를 기반으로 할지 설정 | FROM <이미지>:<태그> |
| MAINTAINER | 이미지를 생성한 사람의 정보를 설정 | MAINTAINER <개인 정보> | 
| USER | 명령을 실행할 사용자 계정을 설정 |
| WORKDIR | `RUN, CMD, ENTRYPOINT`의 명령이 실행될 디렉터리를 설정 |
| RUN | 셸 스크립트 혹은 명령을 실행 | RUN <명령어> |
| VOLUME | 호스트와 공유할 디렉터리 목록 |
| CMD | 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행<br/> `docker run` or `docker start` 명령으로 컨테이너를 시작할 때 실행<br/>CMD는 Dockerfile에서 한 번만 사용 가능 | CMD <명령어> |
| ENTRYPOINT | 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행 |
| WORKDIR | CMD에서 설정한 실행 파일이 실행될 디렉터리 | WORKDIR <디렉터리> |
| EXPOSE | 호스트와 연결할 포트 번호 | EXPOSE <포트번호> |
| ENV | 환경 변수를 설정<br/> `RUN, CMD, ENTRYPOINT` 에 적용 |
| ADD | 파일을 이미지에 추가 |
| COPY | 파일을 이미지에 추가<br/>ADD와는 달리 COPY는 압축 파일을 추가할 때 압축을 해제하지 않고, 파일 URL도 사용할 수 없음|
### Dockerfile로 이미지 build하기
| Command | Description | Format<br/>example |
|:---:|:---|:---|
| build | Dockerfile을 토대로 이미지 생성<br/>--tag 옵션 이용시 이미지 이름과 태그 설정 가능 | sudo docker build <옵션> <Dockerfile 경로><br/>sudo docker build --tag hello:0.1 . |