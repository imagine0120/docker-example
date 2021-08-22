# Docker Tutorial
* [MS 제공 도커 튜토리얼](https://docs.microsoft.com/ko-kr/visualstudio/docker/tutorials/your-application) 로 Docker, Docker Compose 공부하기  
  
### 2부: 애플리케이션
 1. 앱 컨테이너 이미지 빌드
     * Dockerfile : 도커 이미지 빌드에 사용. package.json과 동일한 폴더에 생성.  
          `docker build -t getting-started .`
     * -t(-tag) : 이미지명 지정
     * . : 현재 디렉터리에서 Dockerfile 탐색
     * CMD : 해당 이미지에서 컨테이너 시작 시 실행할 기본 명령 (node, index.js -> node로 서버 기동)  
       
 2. 앱 컨테이너 시작
     * -d : detached mode(분리모드), 즉 데몬 상태로 컨테이너 실행
     * -p : port forwarding. 호스트와 컨테이너 포트 매핑. 애플리케이션에 접근하기 위해 필수 지정  
  
### 3부 : 앱 업데이트
 1. 이전 컨테이너 바꾸기
     * 동일 포트로 새로 빌드한 이미지를 실행한 컨테이너 올리려면 기존 컨테이너 중지 후 재시작 해야 함  
     `docker ps //컨테이너 아이디 확인`  
     `docker stop <container-id>`  
     `docker rm <container-id>`  
  
### 4부 : 앱 공유
 1. 이미지 푸시
     * docker 이미지명은 <registry hostname>/<image name> 으로 이뤄짐. 호스트명은 옵셔널.  
  
### 5부 : 데이터베이스 유지
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
 
### 6부 : 바인드 탑재 사용
 1. 볼륨 유형 빠른 비교  
  
| 속성 | 명명된 볼륨 | 바인딩 마운트  |
|---|:-:|:-:|
| 호스트 위치 | Docker 선택 | 바인딩 마운트 |
| 탑재 예시 | v1:/usr/local/data  | /path/to/data:/usr/local/data  |
|  새 볼륨에 컨테이너 내용 채움 | Y  |  Y |
|  볼륨 드라이버 지원 | Y  |  Y |

    * 바인드 마운트로 컨테이너를 기동하는 명령어를 칠 때, getting-started/app 경로에 있지 않으면 package.json 파일을 찾지 못해서 exit(1) (빌드에러) 난다. 유념하자!


### 7부 : 다중 컨테이너 앱
  1. 다중 컨테이너 앱
      * MySQL을 DB로 todo-app 에 연결하기 위해 좋은 방법은?
      * 별도의 컨테이너를 생성하여 동일한 network에 있도록 묶어주기
      * DB를 별도 컨테이너로 관리한다면 확장성 및 버전 관리, 유지보수 등의 이점  
  2. 컨테이너 네트워킹
      * 네트워크 생성  
      `docker network create <network-name>`  
      * 컨테이너 시작 시 네트워크 설정  
      `docker run --network <nework-name>`  
      `--network-alias <alias> //네트워크 안에서 호스트와 같은 역할`  
  3. MySQL을 사용하여 앱 실행
      * 개발 시엔 환경변수 지정을 통해 DB 연결 및 설정. 그러나 상용에서는 k8s 같은 오케스트레이션 프레임워크에서 제공하는 비밀 데이터 저장 방식이 권장됨
      * MYSQL_HOST : 실행 중인 MySQL 서버 호스트명
      * MYSQL_USER - 연결에 사용할 사용자 이름
      * MYSQL_PASSWORD - 연결에 사용할 암호
      * MYSQL_DB - 연결된 후 사용할 데이터베이스  

### 8부 : Docker Compose 사용
  1. Docker Compose 설치
      * Docker Compose란 다중 컨테이너 애플리케이션을 정의하고 공유하는 툴로, Docker Desktop과 함께 자동으로 설치됨
      * yml 파일로 컨테이너 모두 실행/종료 및 버전 관리 가능
  2. Compose 파일 만들기
      * 앱 프로젝트 루트에 docker-compose.yml 생성
      * `version: "3.7"` : 스키마 버전 정의
      * 
