# The-fastest-joignable-Docker: 도커 공부를 위한 저장소
본 문서는 도커를 다중 사용자의 개발환경 구축을 목적으로 작성되었습니다.
추가되었으면 좋겠다 생각하시는 자료를 알려주시면 반영하도록 하겠습니다.

_본 문서는 아래와 같은 규칙을 따라 작성되었습니다._
- 최종 목적은 <U>개인 맞춤형 Cuda 버전과 파이썬 라이브러리 버전을 지원</U>하기 위함입니다.
- 범용성있는 도커 파일을 구축하는 것이 목적입니다.
- 목적에 맞는 예시와 시행착오 모두 기록합니다.
<br/>

## 과정 및 시행 착오
### 0.공부

- [가장 빨리 만나는 Docker](http://pyrasis.com/docker.html)를 메인 교재로 사용했습니다.
- 빠르게 도커를 사용하시기 위해서는 컨테이너, 이미지의 개념을 잡고 저의 [book_study](https://github.com/embed-Rayn/The-fastest-joignable-Docker/tree/master/book_study)를 참고하시길 추천드립니다.

#### 현 상황 
1. 하나의 GPU 서버의 여러 명이 머신러닝 및 딥러닝을 사용
2. keras에서 GPU를 인식 못하는 이슈가 발생
3. 여러 라이브러리가 서로 꼬여 발생했다고 판단 되고 앞으로 더 들어올 GPU 서버를 관리하기 위한 개발환경 개발 요청

#### 방향
1. 한 서버에서 여러 사용자가 각각 다른 라이브러리를 쓸 수 있는 환경 구축
2. 새로운 사용자가 추가 되었을 때 라이브러리 설치 전까지 자동 환경설정
3. 사용자는 서버의 루트 권한을 부여하지 않아야 함
4. 서버가 추가 되었을 때 쉽게 설정 할 수 있는 확장성 고려

### 1. [계획 1]()
![planA](./planA/PlanA.PNG)
- 컨테이너를 사용자마다(프로젝트 마다) 생성을 합니다.
- 관리자 권한을 사용자에게 줄 수 없으므로 Jupyter notebook으로 접속하도록 합니다.
- 각 Jupyter 셀에 `![COMMAND]`를 이용하여 가상환경에 사용자에 맞는 라이브러리를 설치하도록 합니다.
- CUDA의 경우 로컬에 설치하여 참조하도록 합니다.
<br/>
__[당시 작성한 도커 파일](./Dockerfile_1)__ <br/>
_아래 참고자료에 있는 도커파일에 `pip install` 명령어를 추가하여 실험_ <br/>
_빌드 중 `RUN conda update anaconda`에서 멈추는 경향이 있음_<br/>

#### 1.1. 참고자료 
- Dockerfile 작성 및 이미지 제작
	- 가장 빨리 만나는 Docker의 [도커파일 작성](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04/02), [도커이미지 제작](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04/03), [도커파일 자세히 알아보기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter07)
- Dockerfile 빌드 분석 참고
	- [참고](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)
- anaconda를 설치(기존 RUN apt-get install -r 과 다름)와 jupyter notebook 띄우는 문서
	- http://www.science.smith.edu/dftwiki/index.php/Tutorial:_Docker_Anaconda_Python_--_4

### 2. [계획 2]()
![PlanB](./planB/PlanB.PNG)
- 위의 설계에서는 한 서버에 여러 CUDA 버전을 설치 불가능하다는 생각이 들었습니다.
- CUDA 및 cuDNN을 컨테이너로 올리고 참조하는 방식을 생각했습니다.
- 파이썬 버전, 라이브러리는 가상환경으로 처리합니다.
- 파이썬 라이브러리를 도커 파일 빌드시 같이 설치 할 수 있는 [python 스크립트](./DF_maker.ipynb) 추가

#### 2.1. 참고자료
- [Nvidia docker 설치](https://tobelinuxer.tistory.com/27)

### 3. [계획 3(진행 중)]()
![PlanC](./planC/PlanC.PNG)
- 이전 도커를 사용하려고 했던 목적은 다음과 같습니다.
	- 방향 1: 컨테이너를 생성함으로 서버 로컬의 영향 최소화
	- 방향 2: 도커파일의 설정으로 컨테이너 생성 시 기본 설정 자동화
	- 방향 3: Jupyter notebook 인터페이스를 제공함으로써 서버 CLI 접근 최소화
	- 방향 4: 새로운 서버에 도커를 설치시 쉽게 확장 가능
- 이전 virtualenv를 사용하지 않은 이유는 R언어 및 CUDA의 여러 버전 적용 불가능하다고 판단했습니다.
- 지인분과 대화를 한 결과 서비스 배포 목적이 아닌 개발환경 도커의 적용이 불필요하다고 조언을 받고 다른 방법을 찾았습니다.
	- 방향 1: 가상환경 이용
	- 방향 2: 배치파일과 스크립트를 이용
	- 방향 3: 권한 설정 및 jupyter 인터페이스 제공
	- 방향 4: 검색 중. 배치파일 이용 예정

#### 3.1 참고자료
-[가상환경에 여러 버전의 CUDA 추가](https://blog.kovalevskyi.com/multiple-version-of-cuda-libraries-on-the-same-machine-b9502d50ae77)
-[Linux 가상환경 설치 및 타계정 공유](https://m.blog.naver.com/cjh226/220919371679)