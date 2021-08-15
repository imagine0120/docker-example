# Docker Tutorial
* [MS 제공 도커 튜토리얼](https://docs.microsoft.com/ko-kr/visualstudio/docker/tutorials/your-application) 로 Docker, Docker Compose 공부하기  

### 1부: 애플리케이션
 1. 앱 컨테이너 이미지 빌드
  * Dockerfile : 도커 이미지 빌드에 사용. package.json과 동일한 폴더에 생성.  
       `docker build -t getting-started .`
  * -t(-tag) : 이미지명 지정
  * . : 현재 디렉터리에서 Dockerfile 탐색
  * CMD : 해당 이미지에서 컨테이너 시작 시 실행할 기본 명령 (node, index.js -> node로 서버 기동)  
 2. 앱 컨테이너 시작
  * -d : detached mode(분리모드), 즉 데몬 상태로 컨테이너 실행
  * -p : port forwarding. 호스트와 컨테이너 포트 매핑. 애플리케이션에 접근하기 위해 필수 지정

### 2부 : 앱 업데이트
 1. 이전 컨테이너 바꾸기
  * 동일 포트로 새로 빌드한 이미지를 실행한 컨테이너 올리려면 기존 컨테이너 중지 후 재시작 해야 함  
  `docker ps //컨테이너 아이디 확인`  
  `docker stop <container-id>`  
  `docker rm <container-id>`  

### 3부 : 앱 공유
 1. 이미지 푸시
  * docker 이미지명은 <registry hostname>/<image name> 으로 이뤄짐. 호스트명은 옵셔널.
