---
title: "[Video Summary]Jenkins로 CI/CD 를 구현해보자"
datePublished: Mon Jun 17 2024 03:49:59 GMT+0000 (Coordinated Universal Time)
cuid: clxifsy9l000009jv5zf18qjd
slug: video-summaryjenkins-cicd

---

이번 포스팅에서는 이전에 다룬 **CI/CD와 DevOps** 및 **안드로이드 앱 개발의 CI/CD 개요**를 실제 환경에서 구현해보는 실습을 해보겠습니다. 여기서는 Jenkins에 대해 다루고 다음 포스팅에서 GitHub Actions 를 사용해보겠습니다.

#### 이전 포스팅

* [CI/CD, DevOps의 이해](https://hashnode.com/post/clxe9dwbg000j09l2cuhfb021)
    
* [안드로이드 앱 개발의 CI/CD Overview](https://hashnode.com/post/clxez9ml3000309mfc7gja0aj)
    

이 포스팅은 아래 SKplanet Tacademy의 공개 세미나를 통해 학습한 내용을 정리한 것입니다.

* [\[토크ON세미나\] Jenkins를 활용한 CI/CD 1강 - 젠킨스(Jenkins) 이해 | T아카데미](https://www.youtube.com/watch?v=JPDKLgX5bRg&list=RDCMUCtV98yyffjUORQRGTuLHomw&index=3)
    
* [\[토크ON세미나\] Jenkins를 활용한 CI/CD 2강 - 젠킨스 개발환경 및 CI/CD 기본 동작 이해 | T아카데미](https://www.youtube.com/watch?v=3WZoVkvLE4A)
    
* [\[토크ON세미나\] Jenkins를 활용한 CI/CD 3강 - 젠킨스 CI/CD 파이프라인 구성 실습(1) | T아카데미](https://www.youtube.com/watch?v=GOLHN3FHjpI)
    
* [\[토크ON세미나\] Jenkins를 활용한 CI/CD 4강 - 젠킨스 CI/CD 파이프라인 구성 실습(2) | T아카데미](https://www.youtube.com/watch?v=HVqG59Wtc5c)
    

# 학습 내용

* Jenkins를 사용하여 CI/CD 파이프라인을 구축하는 방법을 실습하였습니다.
    
* AWS EC2에 Jenkins를 설치하고, GitHub와 연동하여 자동화 파이프라인을 구성하였습니다.
    
* 다양한 Jenkins 플러그인들을 활용하여 빌드, 테스트, 배포 과정을 자동화하였습니다.
    
* 최신 Jenkins 설정 과정에서 발생할 수 있는 문제점들을 해결하는 방법을 배웠습니다.
    

# 요약

## 강의 목표

* CI/CD 파이프라인의 기본 개념 이해.
    
* 기본적인 운영환경(DEV, QA, PROD) 이 어떻게 구성되고 운영되는지 이해.
    
* Jenkins 의 기본 개념 이해.
    
* Jenkins 를 통해 기본적인 배포 파이프라인을 직접 구축할 수 있다.
    
* 실제 운영기에서 특히 AWS 기반의 클라우드 환경에서 Jenkins 가 어떻게 활용되는 지 알 수 있다.
    

## CI/CD 란 무엇인가?

Continuous Integration -&gt; 여러 개발자들의 코드 베이스를 계속 통합하는 것

Continuous Delivery -&gt; 사용자, QA, 관리자들에게 서비스를 지속적으로 배달하는 것.

Continuous Deployment -&gt; 코드 베이스를 사용자가 사용가능한 환경에 배포하는 것을 자동화함.

Continuous Deployment 는 많이 다루지 않을 예정.

* 이전에는 롤링 업데이트 등을 위해 많은 개발자가 투입됐지만 현재는 많은 클라우드 서비스에서 해당 기능을 제공함. 이번 강의에선 ECS란 것을 사용할 것.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718517881769/2d886ec4-5d62-4e2a-8529-27fb1c1ba9fa.png align="center")

Jenkins 내에서 빌드와 테스트가 수행됨

빌드 툴로 Webpack, Tsc, javac 등이 잇고

테스팅 툴로 Jest, junit

배포 툴로 ecs update, 쿠버네티스 등이 있음

## CI 가 왜 필요한가요?

10명의 개발자가 열심히 개발

1주 후, 코드 통합 후 에러 발생

1주 간의 코드 컴토 필요

> 자동화 도구로 코드 베이스 통합 과정을 자동화한다.  
> 그 과정에서 코드를 지속적으로 테스트하며 모니터링한다.

## CD 는 왜 필요한가요?

* 1주간 코드 개발
    
* 프론트와 협업해야하니 배포를 해볼까?
    
* 저기 배포좀 해주세요
    
* 앗 버그..! 다시 배포좀 해주세요...
    

> CD 가 없으면 배포 시 개발자가 직접 모든 코드를 검토해야 함.  
> 이 과정에서 문제가 생기면 번거로운 재배포 과정을 다시 밟아야 함.

## Jenkins 의 기본 개념과 동작 방식

### Jenkins 넌 누구냐

* Java Runtime Environment 위에서 동작하는 자동화 서버
    
* 빌드, 테스트, 배포 등 모든 것을 자동화 해주는 자동화 서버!
    
* 다양한 플러그인들을 활용해서 각종 자동화 작업을 처리할 수 있음
    
* 일련의 자동화 작업의 순서들의 집합인 Pipeline 을 통해 CI/CD 파이프라인을 구축함
    

Jenkins 가 도는 서버에 Java Runtime 이 설치되어 있어야 하고 그게 귀찮다고 하면 도커 이미지 배포판을 사용.

Jenkins 는 껍데기이고 여러 플러그인을 조합해서 파이프라인을 구성하여 사용한다.

### 어떤 플러그인이 있냐?

* 정말 많음
    
* 대표적 플러그인들
    
    * Credentials Plugin
        
    * Git Plugin: Jenkins가 GItHub의 한 repo 를 소스코드로 불러와 빌드할 때 사용
        
    * Pipeline (핵심 기능인 파이프라인 마저도 플러그인!)
        
* 처음에는 Recommend 해주는 것을 깔면 어지간한 건 설치됨
    

### Plugin 살펴보기

**Credentials Plugin**

Jenkins 는 단지 서버일 뿐이기 때문에 배포에 필요한 각종 리소스(가령 클라우드 리소스 혹은 베어메탈에 ssh 접근 등)에 접근하기 위해서는 여러가지 중요 정보들을 저장하고 있어야 한다.

이런 중요한 정보(AWS token, Git access token 등)들을 저장해 주는 플러그인

자동으로 공개키 방식의 암호화를 해줌. 그리고 보통은 프라이빗 네트웍에서 운용되기 때문에 외부에서 젠킨스 서버에 접근할 수 없음.

**Pipeline Plugin**

젠킨스의 핵심 기능인 Pipeline을 관리할 수 있게 해주는 플러그인

**Docker Plugin and Docker Pipeline**

Docker agent 를 사용하고 Jenkins 에서 도커를 사용하기 위함

## Pipeline

파이프라인이란 CI/CD 파이프라인을 젠킨스에 구현하기 위한 일련의 플러그인들의 집합이자 구성.

즉 여러 플러그인들을 이 파이프라인에서 용도에 맞게 사용하고 정의함으로써 파이프라인을 통해 서비스가 배포됨.

Pipeline DSL(Domain Specific Language)로 작성

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718519078357/3bd04a68-805a-46dd-8626-ce314a825ed1.png align="center")

## Pipeline을 구성하는 요소

파이프라인이란 CI/CD 파이프라인을 젠킨스에 구현하기 위한 일련의 플러그인들의 집합이자 구성.

즉 여러 플러그인들을 이 파이프 라인에서용도에 맞게 사용하고 정의함으로써 파이프라인을 통해 서비스가 배포됨.

두 가지 형태의 Pipeline syntax 가 존재

* **Declarative Pipeline**
    
* Scripted Pipeline
    

여기선 최신이자 더 가독성이 좋은 Declarative Pipeline Syntax를 사용

## Pipeline Syntax

Sections의 구성

* Agent section
    
* Post section
    
* Stages section
    
* Steps section
    

### Agent section

젠킨스는 많은 일들을 해야 하기 때문에 혼자 하기 버겁다.

여러 slave node 를 두고 일을 시킬 수 있는데, 이처럼 어떤 젠킨스가 일을 하게 할 것인지를 지정한다.

젠킨스 노드 관리에서 새로운 노드를 띄우거나 혹은 docker 이미지들을 통해서 처리할 수 있다.

### Post section

스테이지가 끝난 이후의 결과에 따라서 후속 조치를 취할 수 있다.

Ex) Success, Failure, Always, Cleanup

Ex) 성공 시에 성공 이메일, 실패하면 중단 혹은 건너뛰기 등등.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718519709981/34e5355b-557a-44a7-9a4a-346f85212955.png align="center")

### Stages Section

어떤 일들을 처리할 건지 일련의 stage 를 정의함

### Steps Section

한 스테이지 안에서의 단계로 일련의 스텝을 보여줌

## Pipeline Syntax

Declaratives

Environment, stage, options, parameters, triggers, when 등의 Declarative 가 있음

Environment -&gt; 어떤 pipeline이나 stage scope 의 환경 변수 설정

Parameter -&gt; 파이프라인 실행 시, 파라미터 받음

Triggers -&gt; 어떤 형태로 트리거 되는가

When -&gt; 언제 실행되는가

#### 코드 예시

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718519965936/a4898033-ea81-46e5-878b-e70309fe89a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718519983269/cc30d1ab-2748-4d55-a164-85c5fd14f09c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718520006496/3049646f-cdc4-43ab-b3eb-6d12035b25ee.png align="center")

## Steps

* Steps 내부는 여러가지 스템들로 구성
    
* 여러 작업들을 실행가능
    
* 플러그인을 깔면 사용할 수 있는 스템들이 생겨남
    

플러그인별 스텝 종류

https:www.jenkins.io/doc/pipeline/steps/

플러그인을 하나 설치하면 사용가능한 step 들이 생기게 되는 것

## 젠킨스 설치 방법

젠킨스를 받을 수 있는 yum 업데이트

젠킨스를 받을 수 있는 yum repository 추가

자바 깔기

젠킨스 설치

자바 버전 바꾸기

젠킨스 켜기

## CI/CD 내 여러가지 개발 환경의 종류

* 개발자가 개발을 하는 Local 환경(자신의 workspace 안에서 일을 함)
    
* 개발자들끼리 개발 내용에 대한 통합 테스트를 하는 Development 환경
    
* 개발이 끝나고 QA 엔지니어 및 내부 사용자들이 사용해 보기 위한 QA 환경
    
* 실제 유저가 사용하는 Production 환경
    
* 쉽게 DEV, QA, PROD 환경으로 부른다
    

## 개발 프로세스

1. 개발자가 자신의 PC 에서 개발을 진행한다.
    
2. 다른 개발자가 작성한 코드와 차이가 발생하기 않는지 내부 테스트를 진행한다.
    
    1. 이때 Pre-commit hook을 사용하기도 하는데 이도 CI 의 일부
        
3. 진행한 내용을 다른 개발자들과 공유하기 위해 git 과 같은 SCM 에 올린다. 흔히 dev 브랜치.
    
4. 위 브랜치를 젠킨스가 관찰하고 변경을 개발환경에 배포하기 전에 테스트와 Lint 등 코드 포멧팅을 한다.
    
5. 배포하기 위한 빌드 과정을 거친다
    
6. 코드를 배포한다
    
7. 테스트를 진행한다
    

## 여러 배포 환경의 관리

여러 배포환경의 관리에서 핵심은

**인프라를 모듈화하여 어떤 것이 변수인지 잘 설정하고 이를 잘 설계하는 것**

가령 APP\_ENV 처럼 현재 배포하고자 하는 것이 무슨 환경인지 설정하고 앱 내에서 사용하는 다양한 변수들을 APP\_ENV에 맞게 잘 가져다 쓰는 것이 핵심

#### Dev 와 Prod 에서 바뀌어야 할 게 뭐가 있나?

* 데이터 베이스 패스워드
    
* 서버 URL
    
* 암호화 해시 키
    

서비스 내부의 변수 뿐만 아니라 클라우드 리소스를 많이 활용해서 개발하는 요즘에는 클라우드 리소스 내에서 인프라별 키관리가 매우 중요해서 aws system manager 의 parameter store 와 같은 키 관리 서비스를 쓰는 것을 강추!

## 예시 배포 환경

1. 웹사이트 코드를 작성
    
2. 웹 사이크 코드를 린트, 웹팩 빌드 해서 AWS S3 bucket 에 html 파일을 업로드한다.
    
3. Node.js 백엔드 코드를 typescript 작성한다
    
4. 위 코드를 javascript compile 하고, 테스트 코드를 돌려서 도커 이미지를 만들어 ECR 에 올린다.
    
5. 업로드한 ECR 이미지로 ECS 서비스를 재시작한다. (Rolling deploy) -&gt; Continuous deploy
    

## AWS 리소스 간단 리뷰 - S3

* Simple Storage Service 의 약자로 클라우드 스토리지
    
* 정적 웹사이트 코드 배포에 용이
    
* 정적 웹 사이트 호스팅에 필요한 다양한 기능 제공
    
* AWS Cloudfront 와 함께 사용해서 최적화 가능하고 DNS 관리도 가능
    

## AWS 리소스 간단 리뷰 - ECR

* Elastic Container Registry
    
* 도커 이미지를 저장하는 프라이빗 레포지토리
    

우리가 AWS에서 작업 할 예정이기 때문에 도커 이미지를 만들어서 빌드해 쓰려면 AWS 가 이미지를 가져와 쓸 수 있어야 한다. 이때 docker private repsoitroy 를 사용할 수도 있는데, 여기선 AWS의 ECR에 도커 이미지를 올리고 사용한다.

이럴 때 장점은 필요없는 이미지는 며칠 지나면 지워주거나 배포 시 문제가 생기면 바로 전의 이미지를 가져와 올릴 수 있다.

## AWS 리소스 간단 리뷰 - ECS

* Elastic Container Service
    
* 도커 컨테이너 기반으로 서비스 운용을 가능하게 해주는 간단한 서비스
    
* 내 서비스를 스케일업 가능한 형태로 손쉽게 배포해줌
    
* Rolling Deploy, 로드 밸런싱, 스케일업 다 해줌
    
* 수 많은 도커 컨테이너 서버를 띄우고 LB 가 이들 사이에 밸런싱을 해줌
    
* fargate, ec2 모드가 있어서 docker container 리소스만 띄우거나 혹은 물리적인 EC2 instance 클러스터로 구성 가능
    
    * fargate 는 ec2 인스턴스가 몇 개 올라갔는지 관리자 페이지에서 확인할 수 없다
        
    * ec2 모드는 몇개 인스턴스가 올라갔는지 볼 수 있다
        

젠킨스는 이런 ECS에게 서비스 업데이트해줘 라고 전달한다.

ECS 혹은 k8s 등을 통해 rolling deploy가 처리되기 때문에 jenkins 의 력할은 배포 명령만 내려주면 된다

ex) aws ecs update-service '서비스 이름'

## 개발 환경 예시 1

실제 현업에서는 개발 환경이 운영 환경에 영향을 끼치지 않게 하기 위해서 AWS 계정까지도 달리 쓴다.

이럴때는 jenkins 가 여러개 떠야 한다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718525346613/2ae2a5a3-ca97-4e12-bebc-18434f560056.png align="center")

이런 상황 때문에 요새는 서비스 CI/CD 뿐 아니라 인프라 CI/CD도 사용한다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718525443345/c7d4d6e8-3ac4-492d-a8e7-cf78de9e57fd.png align="center")

## 중간 질문

### Q. 프리커밋 훅을 git clone 받을 때 자동으로 설정할 수 있나요?

모름. 스크립트를 만들고 레포 클론 후 스크립트를 한 번 돌린 후 작업함

## 환경 설정

1. EC2 인스턴스 추가
    
2. pem 키페어 다운
    
3. 키를 ~/.ssh 로 이동
    
4. ~/.ssh 내의 config 파일을 vim 으로 열어 key 를 사용하기 쉽게 설정
    
5. 키의 파일 권한 수정
    
    1. chmod 400 aws-ec2-jenkins1.pem
        
6. EC2 대시보드에서 인스턴스의 보안탭에서 보안 그룹 이동
    
7. 인바운드 규칙 편집 이동
    
8. 80, 8080 포트 개방
    
9. iterm 에서 ssh 접속
    
    1. ssh -i aws-ec2-\[퍼블릭 IPv4 DNS\]
        
    2. ssh jenkins1
        
10. EC2 Amazon Linux 2023 AMI 에서 작업
    
    1. yum 업데이트
        
        1. sudo yum update -y
            
    2. 자바 설치
        
        1. 설치된 자바 버전 확인
            
            1. java -version
                
                1. 이미 설치된 경우(sudo alternatives --config java 로 원하는 버전 설정)
                    
        2. 원하는 버전이 없으면, yum 을 통해 설치 가능한 jdk 검색
            
            1. yum list | grep jdk
                
        3. 현재 yum repository 에 jdk8이 없으므로 aws 문서에서 Amazon Corretto8의 rpm 다운 [(문서)](https://aws.amazon.com/ko/corretto/?filtered-posts.sort-by=item.additionalFields.createdDate&filtered-posts.sort-order=desc)
            
            1. wget [https://corretto.aws/downloads/latest/amazon-corretto-21-x64-linux-jdk.rpm](https://corretto.aws/downloads/latest/amazon-corretto-21-x64-linux-jdk.rpm)
                
        4. 다운 받은 파일 설치
            
            1. sudo yum localinstall -y amazon-corretto-21-x64-linux-jdk.rpm
                
        5. 설치 확인
            
            1. java -version
                
    3. 젠킨스 설치
        
        1. Jenkins 저장소를 yum에 추가
            
            1. sudo wget -O /etc/yum.repos.d/jenkins.repo [https://pkg.jenkins.io/redhat/jenkins.repo](https://pkg.jenkins.io/redhat/jenkins.repo)
                
        2. Jenkins 저장소의 GPG 키를 가져와서 설치
            
            1. sudo rpm --import [https://pkg.jenkins.io/redhat/jenkins.io.key](https://pkg.jenkins.io/redhat/jenkins.io.key)
                
        3. Jenkins 패키지에 필요한 종속성 설치
            
            1. sudo yum install -y fontconfig
                
            2. sudo yum install fontconfig java-17-openjdk
                
        4. Jenkins 설치
            
            1. sudo yum install -y jenkins
                
    4. 서버에서 젠킨스 시작
        
        1. sudo service jenkins start
            
11. 젠킨스 설정 및 활성화
    
    1. 웹 브라우저로 EC2의 퍼블릭 ip 의 8080 포트에 접속
        
    2. 젠킨스의 관리자 페이지를 활성화 하기 위해 어드민 키를 로그에서 찾아 입력
        
        1. sudo cat /var/lib/jenkins/secrets/initialAdminPassword
            
    3. 추천 플러그인 설치
        
    4. 첫 어드민 계정 생성
        
        1. 계정명, 암호, 암호 확인, 이름, 이메일 입력
            
    5. 젠킨스의 루트 주소 설정 후 활성화
        
12. 젠킨스와 Github 연동
    
    1. Jenkins 관리 탭 - Security 섹션에서 Credentials 선택 - System 선택 - Global credentials 선택 - Add Credentials 버튼 선택
        
    2. 연동할 GitHub repo 생성
        
        1. 여기선 테스트 서버와 웹사이트가 존재하는 레포를 fork [(링크)](https://github.com/frontalnh/temp)
            
        2. fork 한 레포의 원래 git 정보 제거 후 fork해온 레포로 연결
            
            1. 현재 연결된 리모트 정보 확인
                
                1. git remote -v
                    
            2. 리모트 변경
                
                1. git remote set-url origin [https://github.com/KeunyoungSong/Jenkins-Test.git](https://github.com/KeunyoungSong/Jenkins-Test.git)
                    
                2. git remote -v (재확인)
                    
    3. GitHub 의 권한을 젠킨스에 부여해야 함
        
        1. GitHub 의 Settings 이동 - Developer Settings - Personal access token - Token(classic) - Generate new token - Token(classic) 선택
            
        2. Repo 권한 선택 후 생성
            
    4. 발급 받은 토큰을 Jenkins 에 등록하기 위해 jenkins 관리자 URL 로 이동
        
        1. Kind 는 Username with password
            
        2. Username 에 github 이름
            
        3. Password 에 token 값
            
        4. ID 는 Jenkins 가 해당 키를 알아볼 수 있게 하는 이름임
            
        5. 등록
            
13. Fork 한 레포지토리의 Jenkinsfile 열기
    
    1. Github 연동을 위한 token 은 어드민 페이지에서 했지만 AWS 연동을 위한 환경 변수 추가 필요
        
    2. AWS 에서 Jenkins를 위해 IAM 에서 사용자 생성
        
    3. 이름 등록 - 권한 설정(여기선 AdministratorAccess을 줌)
        
    4. 사용자 생성 완료
        
    5. 사용자에게 Access Key 추가
        
        1. CLI 권한 부여
            
        2. 엑세스 키, 비밀 엑세스 키 발급
            
14. Jenkins 에 AWS 계정의 access key 등록
    
    1. Jenkins 관리 페이지로 돌아와 다시 New Credentials
        
    2. kind 는 Secret text
        
    3. Secret 은 액세스 키
        
    4. ID 는 Jenkinsfile 에 등록한 id
        
    5. 비밀 access key 도 똑같이 등록(id값에 주의)
        
    6. AWS region 을 알맞게 변경
        
15. Jenkins 의 환경 변수를 어드민 페이지를 통해 등록했으니 이제 Jenkinsfile 로 이동
    
    1. Prepare stage 에서 다운 받을 github repo 주소 변경
        
    2. jenkins 에 등록한 git token id 변경
        
16. 웹 페이지를 S3 에 올리기 위해 AWS 콘솔로 이동
    
    1. S3 인스턴스 생성, 퍼블릭 접근 차단 해제
        
    2. 버깃 정책 변경 후 아래 내용 추가
        
        1. { "Version": "2012-10-17", "Statement": \[ { "Sid": "PublicReadGetObject", "Effect": "Allow", "Principal": "*", "Action": "s3:GetObject", "Resource": "arn:aws:s3:::your-bucket-name/*" } \] }
            
    3. Jenkinsfile 의 s3 인스턴스 이름 변경
        
17. 젠킨스에서 email 을 보내기 위한 설정하기
    
    1. 젠킨스 어드민 페이지 - Jenkins 관리 - System - **E-mail로 알려줌** 섹션이동
        
    2. **SMTP 서버** 에 smtp.gmail.com 입력
        
    3. 고급 클릭 후
        
    4. Use SMTP Authentication, SSL사용 체크
        
    5. Gmail 의 app 비밀번호 발급 후
        
        1. 사용자명에 이메일 계정 id
            
        2. 비밀번호에 app 비밀번호
            
    6. 포트 465
        
    7. Test configuration 수행
        
    8. 저장
        
    9. Jenkinsfile 에서 이메일을 보낼 주소를 모두 알맞게 변경
        
18. Jenkins 플러그인 추가(필수)
    
    1. Jenkins 관리 - Plugins - Available plugins 이동
        
    2. Docker, Docker pipeline 플러그인 설치(git 은 추천 설치 선택했을 때 이미 설치됨)
        
    3. 재시작후 설치 선택
        
19. Jenkins 가 docker 를 사용할 수 있도록 EC2 에 Docker 를 설치
    
    1. sudo yum install docker
        
20. 도커를 사용할 수 있는 그룹에 jenkins user 를 추가
    
    1. sudo usermod -aG docker $USER
        
    2. sudo usermod -aG docker jenkins
        
    3. logout
        
21. 도커 시작
    
    1. EC2 재접속
        
    2. sudo service docker start
        
    3. docker ps
        
22. Jenkins 대시보드 이동
    
    1. 새로운 Item 생성
        
    2. 이름 입력 후 pipeline 선택 후 OK
        
    3. GitHub project 체크 후 레포 주소 입력
        
    4. Poll SCM 체크(설정은 Jenkinsfile 에 함)
        
    5. 스크립트 추가 시(코드 입력, github 의 레포참조) 2가지 방식 가능
        
        1. Jenkinsfile 코드 복붙
            
        2. Pipeline script from SCM
            
            1. 설정 맞게 등록
                
23. 생성한 item 탭 이동
    
    1. 지금 빌드 선택
        
    2. 해당 빌드로 가 Console output, Pipeline Overview 등 확인 가능
        
24. QA, DEV, PROD에서 Jenkins 머신 마다 ENV 을 설정하는게 중요
    
    1. Jenkins 관리 탭 - System - Global properties 섹션 - Environment variables 체크
        
    2. 키설정 방법 검색 필요
        
25. 빌드 했는데 "**Waiting for next available executor"** 가 계속 유지됨
    
    1. Jenkins 관리 - System Configuration - Nodes 이동
        
    2. 노드가 오프라인임
        
    3. 스왑 메모리와 메모리를 요구사항 만큼 올려줌
        
    4. 용량을 늘리고 다시 sudo systemctl start jenkins
        
26. 젠킨스 빌드 시작 성공
    
27. 사용법
    
    1. sudo systemctl start jenkins
        
    2. sudo systemctl restart jenkins
        
    3. sudo systemctl stop jenkins
        

## 젠킨스 빌드 중 이슈

### Prepare 단계 에러

**Jenkins 가 git clone 을 하는데 ec2 서버에 git 설치가 되어있지 않음**

```kotlin
Cloning the remote Git repository
Cloning repository https://github.com/KeunyoungSong/Jenkins-Test.git
 > git init /var/lib/jenkins/workspace/Jenkins-Test@2 # timeout=10
ERROR: Error cloning remote repo 'origin'
hudson.plugins.git.GitException: Could not init /var/lib/jenkins/workspace/Jenkins-Test@2
...
Caused by: java.io.IOException: Cannot run program "git" (in directory "/var/lib/jenkins/workspace/Jenkins-Test@2"): error=2, No such file or directory
```

1. git --version
    
2. sudo yum install git -y
    
3. which git
    
4. Jenkins 관리 탭 - Tools - Git installations 이동
    
5. 경로로 which git 해서 나온 결과 등록
    
6. 저장
    

### Deploy Frontend 단계 에러

**Gmail SMTP 설정 문제로 결과 전송 email 이 보내지지 않음**

1. SMTP 설정 시 gmail 의 비밀번호가 아닌 발급받은 app 패스워드를 사용해야 함
    

## 환경 구축 시 최신 변동 사항

* 레포등록 후 Java 를 다운 받는 것이 아닌 aws 에서 제공하는 java 를 다운
    
* Amazon Linux2 는 더이상 Jenkins 프로젝트에서 지원하지 않음 [(참고)](https://community.jenkins.io/t/nokey-error-while-using-terraform-on-aws/15890/2)
    
    * Amazon Linux 2023, Red Hat Enterprise Linux 9, AlmaLinux 9 또는 Rocky Linux 9와 같은 최신 운영 체제를 사용해야된다고 함.
        
* 영상에서는 java 8 로 jenkins 를 설치했지만 최신 Jenkins 를 사용하려면 17 또는 21이 필요했다.[(문서)](https://www.jenkins.io/doc/book/platform-information/support-policy-java/)
    
* EC2 에 Jenkins 를 올리면 메모리 관련 설정을 해주어야 한다. 그렇지 않으면 빌드되지 않는다.
    
* Jenkins 에서 git clone 작업을 하기 때문에 ec2 서버에 git을 설치해주어야 한다. 플러그인 만으로 되지 않음
    

## Jenkins 파이프라인 자동화 내역

1. **파이프라인 개요**
    
    * Jenkins를 사용하여 코드를 주기적으로 빌드하고, 테스트하고, 배포하는 자동화된 CI/CD 파이프라인.
        
2. **Agent 설정**
    
    * 사용 가능한 아무 Jenkins 에이전트에서 실행.
        
3. **트리거 설정**
    
    * SCM 변화를 3분마다 체크하여 실행.
        
4. **환경 변수 설정**
    
    * AWS 접근 키와 비밀 접근 키, 기본 지역 설정 등 파이프라인 내에서 사용할 환경 변수 설정.
        
5. **Prepare 스테이지**
    
    * **목적**: 소스 코드를 다운로드.
        
    * **작업**: 레포지토리를 클론.
        
    * **후속 작업**:
        
        * 성공 시: 레포지토리 다운로드 성공 로그.
            
        * 항상: 작업 시도 로그.
            
        * 정리 작업: 모든 후속 작업 후 로그.
            
6. **Deploy Frontend 스테이지**
    
    * **목적**: 프론트엔드 파일을 AWS S3에 배포.
        
    * **작업**: 정적 파일들을 S3에 업로드.
        
    * **후속 작업**:
        
        * 성공 시: 성공 이메일 발송.
            
        * 실패 시: 실패 이메일 발송.
            
7. **Lint Backend 스테이지**
    
    * **목적**: 백엔드 코드를 린트.
        
    * **작업**: Docker를 사용하여 Node.js 환경에서 린트 작업 수행.
        
8. **Test Backend 스테이지**
    
    * **목적**: 백엔드 테스트 실행.
        
    * **작업**: Docker를 사용하여 Node.js 환경에서 테스트 코드 실행.
        
9. **Build Backend 스테이지**
    
    * **목적**: 백엔드 코드 빌드.
        
    * **작업**: Docker 이미지를 빌드.
        
    * **후속 작업**:
        
        * 실패 시: 파이프라인 중단.
            
10. **Deploy Backend 스테이지**
    
    * **목적**: 백엔드 애플리케이션 배포.
        
    * **작업**: Docker 컨테이너 실행.
        
    * **후속 작업**:
        
        * 성공 시: 성공 이메일 발송.
            

# 메모

1. JRE(Java Runtime Environment): Java 애플리케이션을 실행하기 위한 환경. JVM(Java Virtual Machine), 코어 클래스, 지원 라이브러리등을 포함. 개발 도구는 포함되지 않는다.
    
2. JDK(Java Development Kit): JRE 뿐 아니라 Java 애플리케이션 개발을 위한 도구(컴파일러, 디버거, etc.)도 포함.
    
3. Amazon Corretto는 무료로 사용할 수 있는 Open Java Development Kit(OpenJDK)의 프로덕션용 멀티플랫폼 배포판
    
4. which java
    
    1. java 명령이 위치한 디렉토리 경로를 알려줌
        
5. JDK 는 JRE 를 포함함
    

# 마무리

이번 포스팅에서는 Jenkins를 AWS EC2 환경에 설치하고, CI/CD 파이프라인을 구축하여 안드로이드 앱 개발의 자동화를 실습해보았습니다. Jenkins의 설치부터 시작하여, GitHub와 연동하고, 다양한 플러그인들을 활용하여 빌드와 배포 파이프라인을 구성하는 과정을 자세히 다뤘습니다.

이제 Jenkins를 사용하여 실제 환경에서 CI/CD를 구현하는 방법을 이해했으므로, 다음 포스팅에서는 GitHub Actions를 사용한 CI/CD 구현 방법에 대해 다뤄보겠습니다.

GitHub Actions는 GitHub에서 제공하는 CI/CD 도구로, Jenkins와는 다른 방식으로 자동화를 제공하며, 이를 통해 다양한 CI/CD 도구들을 비교하고 학습할 계획힙니다.

---

젠킨스 실행시 관리자 페이지에서 볼 수 있는 귀여운 젠킨스

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718617464122/1fa27178-c227-44e7-ba5c-1c9048d1849c.png align="center")

아래와 같이 성공한 빌드를 확인할 수 있다

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718617512645/ebd216d1-3ccb-4cbf-9ce1-f65e13ee6f15.png align="center")

블루오션 플러그인을 사용하면 이미지와 함께 상태를 확인 할 수 있다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718617542855/01c6408f-be85-4dc5-805c-ecf73679a24e.png align="center")

성공적으로 S3 에 배포된 정적 웹페이지

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718617726414/85adce3f-0e77-4af3-9212-39b0d0314361.png align="center")

각 스텝별로 경과를 메일로 받은 내역

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718617675788/3475cc50-fbfc-44d6-bfff-35b9580f5486.png align="center")