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
  
### 4부 : 데이터베이스 유지
 1. 컨테이너 파일 시스템
     * 동일 이미지로 컨테이너 실행해도 별개의 파일시스템/데이터 가짐
     `docker exec <container-id> [명령어] //외부에서 컨테이너 안의 명령어 실행`
     * shuf : 숫자 랜덤 추출하는 리눅스 명령어. -i 옵션으로 범위, -n 옵션으로 추출할 개수 지정. -o 옵션으로 추출한 값을 파일로 저장
     * tail : 파일 출력 리눅스 명령어. -f 옵션으로 마지막 10 line 실시간 출력 가능  
   
 2. 컨테이너 볼륨
     * 컨테이너 = 격리된 환경 : 컨테이너는 호스트 머신에서 격리된 실행 환경을 가지기에, 변경내용도 그 안에만 적용됨
     * 볼륨 : 컨테이너의 특정 파일 시스템 경로를 호스트 머신에 연결하고 파일을 저장하면, 컨테이너를 다시 시작해도 해당 파일을 볼 수 있음  
 
 3. todo 데이터 유지
   * 볼륨 생성 후, 이를 컨테이너 내부의 디렉터리에 마운트
   `docker volume create <volume-name> //볼륨 생성`
   `docker run -dp <host-port>:<container-port> -v <volume-name>:<container-directory> <image-name>  
  
 4.  볼륨 자세히 살펴보기
   * docker volume inspect <volume-name> : 볼륨 데이터 저장 경로 등 확인 가능
 
### 5부 : 바인드 탑재 사용
 
