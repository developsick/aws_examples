# AWS 서비스를 활용한 K8S 기반 Web Application Deployment 자동화

## Part 1. Setup AWS services

### 01. AWS 사용자 및 IAM 설정

* IAM 서비스로 이동
![01_01](/images/01_01.png)
* 서비스 > 사용자 추가 이동
![01_02](/images/01_02.png)
* 사용자 추가 기본 정보 입력
    * 프로그래밍 방식 엑세스 / AWS Management Console 액세스 체크
![01_03](/images/01_03.png)
* 권한 설정
    * Backend에서 연결시 Full Access를 위해 'Administrator Access' 추가
![01_04](/images/01_04.png)
* 태그 설정은 생략함
* 사용자 추가 입력항목 검토 후 사용자 생성
![01_05](/images/01_05.png)
* 사용자 생성 후 생성한 user에 대한 임시 패스워드 및 모계정에 연동된 console link가 포함된 credential.csv 파일 다운로드가 가능하다
![01_06](/images/01_06.png)

### 02. Backend Repository 생성 (Codecommit)

### 03. Backend Server Database 생성 (RDS - Mariadb)

### 04. Backend Source Push

### 05. Docker Repository 생성 (ECR)

### 06. Kubernetes Cluster 생성 (EKS)

### 07. Build Pipeline 생성 (Codebuild)

### 08. 서비스 확인

-------------------------------------------------------------

## Part 2. Implement backend source code