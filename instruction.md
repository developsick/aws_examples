# AWS 서비스를 활용한 K8S 기반 Web Application Deployment 자동화

[Part 1. Setup AWS services](#part-1.-setupaws-services)
* [1-01. AWS 사용자 및 IAM 설정](#01.-AWS-사용자-및-IAM-설정)   
* [1-02. Backend Repository 생성 (Codecommit)](#02.-Backend-Repository-생성-(Codecommit))       
* [1-03. Backend Server Database 생성 (RDS-Mariadb)](#03.-Backend-Server-Database-생성-(RDS-Mariadb))    
* [1-04. Docker Repository 생성 (ECR)](#04.-Docker-Repository-생성-(ECR))    
* [1-05. Kubernetes Cluster 생성 (EKS)](#05.-Kubernetes-Cluster-생성-(EKS))    
* [1-06. Build Pipeline 생성 (Codebuild)](#06.-build-pipeline-생성-(codebuild))    
     
[Part 2. Implement backend source code](##part-2.-implement-backend-source-code)


-------------------------------------------------------------
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
* CodeCommit 서비스로 이동
![02_01](/images/02_01.png)
* 리포지토리 > 리포지토리 생성 이동
![02_02](/images/02_02.png)
* 리포지토리 이름 / 설명 입력 후 생성
![02_03](/images/02_03.png)
* 리포지토리 생성 확인 후 연결방법 확인
![02_04](/images/02_04.png)
* 리포지토리 생성 후 리포지토리 리스트에서 생성한 리포지토리 확인
![02_05](/images/02_05.png)

### 03. Backend Server Database 생성 (RDS-Mariadb)
* RDS 서비스로 이동
![03_01](/images/03_01.png)
* 데이터베이스 > 데이터베이스 생성 이동
![03_02](/images/03_02.png)
* 데이터베이스 생성을 위한 설정값 입력 후 데이터베이스 생성
    * 단순 환경 설정을 위해 템플릿에서 '프리 티어' 선택
![03_03](/images/03_03.png)
![03_04](/images/03_04.png)
![03_05](/images/03_05.png)
![03_06](/images/03_06.png)
![03_07](/images/03_07.png)
* 생성 완료 후 생성된 데이터베이스 리스트에서 확인
![03_08](/images/03_08.png)

### 04. Docker Repository 생성 (ECR)
* ECR 서비스로 이동
![04_01](/images/04_01.png)
* ECR > Repositories > 리포지토리 생성 이동
![04_02](/images/04_02.png)
* 리포지토리 이름 입력 후 리포지토리 생성
![04_03](/images/04_03.png)
* 리포지토리 생성 후 리포지토리 리스트에서 생성한 리포지토리 확인
![04_04](/images/04_04.png)

### 05. Kubernetes Cluster 생성 (EKS)
* EKS 서비스로 이동
![05_01](/images/05_01.png)
* EKS > 클러스터 > 클러스터 생성 이동
![05_02](/images/05_02.png)
* 클러스터 이름 입력 후 클러스터 서비스 역할 생성을 위해 IAM 콘솔로 이동
![05_03](/images/05_03.png)
* IAM > 엑세스 관리 > 역할 > 역할 만들기 이동
![05_04](/images/05_04.png)
* 역할을 만들기 위한 사용 사례 선택
    * 신뢰할 수 있는 유형의 개체 : AWS 서비스 서비스
    * 사용 사례 (일반 사용 사례) : EC2 선택
* 각 서비스 선택 후 권한 설정으로 이동
![05_05](/images/05_05.png)
* 필터에 'AmazonEKS' 입력 후 검색되는 정책 중 아래 두개 선택 후 태그 설정 없이 검토로 이동
    * AmazonEKSClusterPolicy
    * AmazonEKSServicePolicy
![05_06](/images/05_06.png)
* 역할 이름 입력 후 역할 생성
![05_07](/images/05_07.png)
* 역할 생성 후 리스트에서 생성한 역할 확인
![05_08](/images/05_08.png)
* 다시 클러스터 구성 화면으로 돌아와서 클러스터 서비스 역할에 생성한 역할 선택 후 다음으로 이동
![05_09](/images/05_09.png)
* 이후 설정 화면에서 설정값 변경 없이 이동하면서 클러스터 생성

* 생성 이후 클러스터 리스트에서 생성한 클러스터 확인
![05_10](/images/05_10.png)


-------------------------------------------------------------

## Part 2. Implement backend source code