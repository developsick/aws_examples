# AWS 서비스를 활용한 K8S 기반 Web Application Deployment 자동화

## Part 1. Setup AWS services

### 01. AWS 사용자 및 IAM 설정
* IAM 서비스로 이동   
![part1_01_01](/images/part1/01_01.png)
* 서비스 > 사용자 추가 이동    
![part1_01_02](/images/part1/01_02.png)
* 사용자 추가 기본 정보 입력
    * 프로그래밍 방식 엑세스 / AWS Management Console 액세스 체크    
![part1_01_03](/images/part1/01_03.png)
* 권한 설정
    * Backend에서 연결시 Full Access를 위해 'Administrator Access' 추가    
![part1_01_04](/images/part1/01_04.png)
* 태그 설정은 생략함
* 사용자 추가 입력항목 검토 후 사용자 생성    
![part1_01_05](/images/part1/01_05.png)
* 사용자 생성 후 생성한 user에 대한 임시 패스워드 및 모계정에 연동된 console link가 포함된 credential.csv 파일 다운로드가 가능하다     
![part1_01_06](/images/part1/01_06.png)

### 02. Backend Repository 생성 (Codecommit)
* CodeCommit 서비스로 이동    
![part1_02_01](/images/part1/02_01.png)
* 리포지토리 > 리포지토리 생성 이동    
![part1_02_02](/images/part1/02_02.png)
* 리포지토리 이름 / 설명 입력 후 생성    
![part1_02_03](/images/part1/02_03.png)
* 리포지토리 생성 확인 후 연결방법 확인    
![part1_02_04](/images/part1/02_04.png)
* 리포지토리 생성 후 리포지토리 리스트에서 생성한 리포지토리 확인    
![part1_02_05](/images/part1/02_05.png)

### 03. Backend Server Database 생성 (RDS-Mariadb)
* RDS 서비스로 이동    
![part1_03_01](/images/part1/03_01.png)
* 데이터베이스 > 데이터베이스 생성 이동    
![part1_03_02](/images/part1/03_02.png)
* 데이터베이스 생성을 위한 설정값 입력 후 데이터베이스 생성
    * 단순 환경 설정을 위해 템플릿에서 '프리 티어' 선택    
![part1_03_03](/images/part1/03_03.png)
![part1_03_04](/images/part1/03_04.png)
![part1_03_05](/images/part1/03_05.png)
![part1_03_06](/images/part1/03_06.png)
![part1_03_07](/images/part1/03_07.png)
* 생성 완료 후 생성된 데이터베이스 리스트에서 확인    
![part1_03_08](/images/part1/03_08.png)

### 04. Docker Repository 생성 (ECR)
* ECR 서비스로 이동      
![part1_04_01](/images/part1/04_01.png)
* ECR > Repositories > 리포지토리 생성 이동     
![part1_04_02](/images/part1/04_02.png)
* 리포지토리 이름 입력 후 리포지토리 생성     
![part1_04_03](/images/part1/04_03.png)
* 리포지토리 생성 후 리포지토리 리스트에서 생성한 리포지토리 확인     
![part1_04_04](/images/part1/04_04.png)

### 05. Kubernetes Cluster 생성 (EKS)
* EKS 서비스로 이동    
![part1_05_01](/images/part1/05_01.png)
* EKS > 클러스터 > 클러스터 생성 이동    
![part1_05_02](/images/part1/05_02.png)
* 클러스터 이름 입력 후 클러스터 서비스 역할 생성을 위해 IAM 콘솔로 이동    
![part1_05_03](/images/part1/05_03.png)
* IAM > 엑세스 관리 > 역할 > 역할 만들기 이동    
![part1_05_04](/images/part1/05_04.png)
* 역할을 만들기 위한 사용 사례 선택
    * 신뢰할 수 있는 유형의 개체 : AWS 서비스 서비스
    * 사용 사례 (일반 사용 사례) : EC2 선택
* 각 서비스 선택 후 권한 설정으로 이동    
![05_05](/images/part1/05_05.png)
* 필터에 'AmazonEKS' 입력 후 검색되는 정책 중 아래 두개 선택 후 태그 설정 없이 검토로 이동     
    * AmazonEKSClusterPolicy
    * AmazonEKSServicePolicy     
![05_06](/images/part1/05_06.png)
* 역할 이름 입력 후 역할 생성    
![05_07](/images/part1/05_07.png)
* 역할 생성 후 리스트에서 생성한 역할 확인    
![05_08](/images/part1/05_08.png)
* 다시 클러스터 구성 화면으로 돌아와서 클러스터 서비스 역할에 생성한 역할 선택 후 다음으로 이동    
![05_09](/images/part1/05_09.png)
* 이후 설정 화면에서 설정값 변경 없이 이동하면서 클러스터 생성

* 생성 이후 클러스터 리스트에서 생성한 클러스터 확인    
![part1_05_10](/images/part1/05_10.png)


-------------------------------------------------------------
## Part 2. Implement backend source code

### 01. Spring Templete 생성
* 기본 IDE로 IntelliJ 사용
* 실행 후 New Project 선택       
![part2_01_01](/images/part2/01_01.png)   
* Project Templete에서 Spring Initializr 선택        
![part2_01_02](/images/part2/01_02.png)    
* Group명 입력 / Java version 선택
    * Group: com.app
    * Java Version: 8    
![part2_01_03](/images/part2/01_03.png)    
* Templete 생성시 사용할 Dependencies 선택    
    * Developer Tools
        * Lombok
    * Security
        * Spring Security
    * SQL
        * Spring Data JDBC
        * MyBatisFramework
    * Amazon Web Services
        * AWS RDS    
![part2_01_04](/images/part2/01_04.png)    
* Project Name 입력 후 Templete 생성    
![part2_01_05](/images/part2/01_05.png)    

### 02. source 초기 세팅
* 생성한 templete에 다음 디렉토리 / 파일을 추가
    * Directories
        * /k8s: EKS에 deploy시 사용할 yaml 파일
        * /src/main/resources/mapper: Mybatis를 사용해 서비스 DB(RDS)에 CRUD를 할 Query 파일
        * /src/main/resources/profiles: 서버 start시 사용할 설정 파일
        * /src/main/resources/profiles/aws: EKS에 deploy한 서버 기동시 사용할 설정 파일
        * /src/main/resources/profiles/local: local에서 서버 기동시 사용할 설정 파일
    * Files
        * /k8s/app.deploy.yaml: deployment 설정 파일
        * /k8s/app.service.yaml: service 설정 파일 
        * /k8s/app.ingress.yaml: ingress 설정 파일
        * /src/main/resources/mapper/UserMapper.xml: USER 테이블 관련 Query 저장 파일
        * /src/main/resources/profiles/local/application.properties: 서버 기동시 사용되는 property 저장 파일
        * /src/main/resources/profiles/local/aws.yml: AWS 연결 정보 및 AWS 서비스 접근시 사용되는 credential 정보
        * /src/main/resources/profiles/local/logback-spring.xml: logback을 통한 log 저장 설정 파일
        * /Dockerfile: 도커 이미지 빌드 파일
        * buildspec.yml: AWS Codebuild를 통한 build/deploy 설정 파일    
![part2_02_01](/images/part2/02_01.png)

### 03. source 구현
* TCL 조기 종료로 다음 phase로 일정 변경

-------------------------------------------------------------
## Part 3. Build / Deploy app to EKS using Codebuild

### 01. DockerFile 생성
```dockerfile
FROM openjdk:8-jdk-alpine

ENV JAVA_OPTS=""
ENV DOC_ROOT /app
ENV VER=1.0
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en

RUN mkdir -p /root/.ssh
RUN chmod 700 /root/.ssh
RUN mkdir -p /Logs

ADD ./target/app-1.0.jar /app/app-1.0.jar

USER root
ENV TZ 'Asia/Seoul'
RUN echo $TZ > /etc/timezone

EXPOSE 8080
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app/app-1.0.jar"]
``` 

### 02. Buildspec.yml 생성
* AWS console > IAM > 생성한 사용자 계정 > 보안 자격 증명 이동
![part3_02_01](/images/part3/02_01.png)
* 엑세스 키 > 엑세스 키 만들기

* 생성된 엑세스 키의 Access key / Secret key를 확인
  (.csv파일을 저장해두지 않으면 이후 다시 확인할 방법 없이 재생성 필요)   
![part3_02_02](/images/part3/02_02.png)
  
### 03. Buildspec.yml 생성
 * 각 단계 설명
    * install: java runtime 및 aws 접속 정보 설정
        * 앞서 생성한 Access key / Secret key 사
    * pre_build: aws 접속정보로 ecr 로그인 여부 확인
    * build: docker image 생성 및 tag 변경
    * post-build: 생성한 docker image ecr에 push후 eks에 해당 이미지 deploy 및 service/ingress 설정
 * 참고사항
    * 배포 환경에 따른 buildspec.yml 변경 최소화를 위해 필요한 값을 변수처리함
    * 변수는 이후 Codebuild에서 설정     
```yaml
version: 0.2

cache:
  paths:
    - '/root/.m2/**/*'
phases:
  install:
    runtime-versions:
      java: openjdk8
    commands:
      - aws configure set aws_access_key_id $KEY_ID --profile $EKS_PROFILE
      - aws configure set aws_secret_access_key $SECRET_KEY --profile $EKS_PROFILE
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - echo $KEY_ID
      - echo $SECRET_KEY
      - $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)
  build:
    commands:
      - echo Build started on `date`
      - echo Building the source code using maven #
      - mvn clean package -DskipTests -P $PROFILE
      - echo Building the Docker image...
      - echo $PROFILE
      - docker build --build-arg PROFILE=$PROFILE -t $IMAGE_REPO_NAME:$IMAGE_TAG .
      - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker image...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
      - aws eks --region $AWS_DEFAULT_REGION update-kubeconfig --name $EKS_CLUSTER --profile $EKS_PROFILE
      - kubectl apply -n $EKS_NAMESPACE -f k8s/app-deploy.yaml
      - kubectl apply -n $EKS_NAMESPACE -f k8s/app-service.yaml
      - kubectl apply -n $EKS_NAMESPACE -f k8s/app-ingress.yaml
      - kubectl -n $EKS_NAMESPACE scale deploy $EKS_DEPLOY --replicas=0
      - kubectl -n $EKS_NAMESPACE scale deploy $EKS_DEPLOY --replicas=$POD_REPLICAS
```
    
### 04. Codebuild 설정
* 