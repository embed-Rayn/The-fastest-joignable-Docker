# The-fastest-joignable-Docker: 도커 공부를 위한 저장소
본 문서는 도커를 공부하는 분들을 대상으로 작성되었습니다.
추가되었으면 좋겠다 생각하시는 자료를 알려주시면 반영하도록 하겠습니다.

_본 문서는 아래와 같은 규칙을 따라 작성되었습니다._
- [가장 빨리 만나는 Docker](http://pyrasis.com/docker.html)를 바탕으로 정리합니다.
- 저의 개념에 따라 목차가 다소 변경될 수 있습니다.
- 최종 목적은 <U>개인 맞춤형 Cuda 버전과 파이썬 라이브러리 버전을 지원</U>하기 위함입니다.
<br/>

## 2.도커 설치하기
[링크 대체](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter02)
## 3.도커 명령어
Docker의 명령은 docker <명령>
항상 root 권한(sudo)으로 실행
### 이미지 관련 명령어
| Command | Description | Format<br/>example |
|---|---:|:---:|
| images | 보유한 이미지의 목록 출력 | sudo docker images |
| search | Docker Hub에서 이미지를 검색 | sudo docker search <검색할 이미지><br/>sudo docker search ubuntu |
| pull | Docker Hub에서 이미지를 다운로드 | sudo docker pull <이미지 이름>:<태그><br/>sudo docker pull ubuntu:latest |
| run | 이미지를 컨테이너로 생성한 뒤 Bash 셸을 실행 | sudo docker run <옵션> <이미지 이름> <실행할 파일><br/>sudo docker run -it --name hello ubuntu /bin/bash |
| rmi | 이미지 삭제 | sudo docker rmi <이미지 이름>:<태그><br/>sudo docker rmi ubuntu:latest |

*<이미지 이름> 대신 이미지 ID를 사용해도 됩니다.* <br/>
*태그를 생략할 경우 pull은 latest 태그만 다운로드, rmi는 모든 태그를 삭제합니다.* <br/>

### 컨테이너 관련 명령어
| Command | Description | Format |
|:---:|:---|:---|
| ps | 모든 컨테이너 목록을 출력<br/>-a 옵션 이용시 종료돈 컨테이너 목록도 출력 | sudo docker ps <옵션><br/>sudo docker ps -a |
| start | 정지된 컨테이너 시작 | sudo docker start <컨테이너 이름><br/>sudo docker start hello |
| restart | 컨테이너 재시작 | sudo docker restart <컨테이너 이름><br/>sudo docker restart hello |
| attach | 백그라운드에서 시작한 컨테이너 접속 | sudo docker attach <컨테이너 이름><br/>sudo docker attach hello |
| exec | 백그라운드에서 실행된 컨테이너의 명령 실행 | sudo docker exec <컨테이너 이름> <명령> <매개 변수><br/>sudo docker exec hello echo "Hello World" |
| stop | 컨테이너 정지 | sudo docker stop <컨테이너 이름><br/>sudo docker stop hello |
| rm | 컨테이너 삭제 | sudo docker rm <컨테이너 이름><br/>sudo docker rm hello |

*<컨테이너 이름> 대신 컨테이너 ID를 사용해도 됩니다.* <br/>

### 컨테이너 Bash 셸 내부 명령어
| Command | Description |
|:---:|:---|
| exit or ctrl + d | 컨테이너를 stop 후 빠져나옴 |
| ctrl + q, ctrl + p | 컨테이너를 정지하ㅏ지 않고 Bash 셸 나옴 |
