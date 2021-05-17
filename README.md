# AWS


- ## IAM
    - 리소스에 대한 액세스 관리  
    - 역할을 가지게 될 __서비스 주체__ 와 그 서비스 주체에 어떤 역할을 부여할지에 대한 __정책__  
    - 즉, 내가 서비스할 주체에 대한 정책을 설정하는 곳  

- ## EC2
    - One Instance = One Computer
    - **AMIs**
        - 아마존 머신 이미지 => 현재의 인스턴스의 상태를 그대로 저장해서 복원할수 있는 시스템  
        - 깃허브의 브랜치와 비슷한 개념

    - **Elastic IP**
        - 고정 IP 할당

    - **Scalability(확장성)**
        - 변화하는 수요에 탄력적인 변화 가능
        - Scale Up , Scale Down
            - 1. 사용중인 인스턴스의 이미지를 만든다 (이 경우 인스턴스가 running이 아니기에 사용자들에게 미리 알려야 함)
            - 2. Scale up 한 새로운 인스턴스를 이미지를 가지고 생성
            - 3. Elastic IP 를 새로 만든 인스턴스에 할당
        - **Scale Out**
            - 컴퓨터의 속도가 무료로 빨라지는 시대는 끝남
            - Scale Up으로 인해 더이상 컴퓨터의 속도가 늘어 날 수 없다면 여러개의 컴퓨터를 사용해야 함
            - <img width="646" height="368" alt="Screen Shot 2021-01-07 at 2 05 37 AM" src="https://user-images.githubusercontent.com/56465854/103853496-11781980-5063-11eb-85c0-ceb321bf5f02.png">


    - **ELB(Elastic Load Balancers)**
        - 유저들의 요청을 자동으로 분산 시켜 웹에 전달 
        - 하나의 로드밸런서에 여러개의 Instance를 묶으면 ELB가 자동으로 분산 시켜 전달 해줌
        - Health Check : Instance들의 연결과 끊김 여부를 확인
        
    - **동시접속 스트레스 테스트**
        - 웹서버에 접속자가 400명이고 200명 동시 접속
        - <img width="1121" alt="Screen Shot 2021-01-06 at 7 41 28 AM" src="https://user-images.githubusercontent.com/56465854/103853657-7b90be80-5063-11eb-9cf6-88fe7a0e30d1.png">

    - **Error**   
        - [권한거부(public key) err Handling ](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html#TroubleshootingInstancesConnectingPuTTY)  

    - **Auto Scaling**
        - 로드 밸런싱을 자동으로 해주는 시스템
        - 내가 가진 인스턴스 이미지와 ELB를 설정
        - CPU 점유율, 퍼센테이지 , 알람 ,부하가 걸릴 때 인스턴스를 늘리고 줄이기 등등 조정 가능
        - 클라우드에서 설정된 전체적인 요금이나 알람 등은 Cloud Watch 에서 확인 

- ## 배포
    - **현재위치배포**
        - 인스턴스를 하나하나 줄여가며 새로운 버전을 배포하는 방식
    - **블루/그린배포**
        - AutoScaling Group 별로 배포하는 방식
    - **배포 자동화**
        - CodeDeploy
            - AppSpec.yml 을 소스코드에 추가해 배포를 자동화하는 방식



## DNS  
![image](https://user-images.githubusercontent.com/56465854/112920746-8b8cdb80-9144-11eb-8c5b-5b1d547153a6.png)  

- **Route53** 
    - 역할 
        - 등록대행자와 네임서버를 임대
            - 1.도메인 구입 (등록대행자(Registrar)로 사용 하기) 
                - Register Domain 에서 원하는 이름의 도메인을 입력하고 구매 시작
            - 2.호스팅 영역에 있는 게 Namer server (새로 만들면 생성이 되어있음)
- route53이 아닌 다른 등록대행자(가비아,고대디 등)를 사용했다면 이 name server를 전세계에 알려야 함
    - 1.등록된 NameServer를 등록대행자 값에 넣어줌 -> 등록소(Registry)에 Top level domin ex) .com , .net 으로 검색하면 따라가게 만듬

- **Record 등록**  
    - A레코드 : Domain Name과 IP를 연결  
    - CNAME : Domain Name과 Domain Name을 연결  
    - A레코드의 Alias : 엘라스틱 빈스톡,s3, ec2 인스턴스등의 별칭으로 설정  

- ## Elastic Beanstalk
    - [Install on MacOS](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-install-osx.html) 
        -  brew update
        -  brew install awsebcli
        -  eb --version (check)
    - Updgrade with pip
        - pip install --upgrade awsebcli
    - [eb init](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-configuration.html)
        - 리전 선택 
        - 액세스, 보안 키 입력 ( IAM 으로 생성 ) 
        - IAM -> 액세스 관리 -> 사용자 -> 보안 자격 증명 -> 액세스 키 만들기 -> .aws/credentials 안에 
        - ![스크린샷 2021-05-17 오후 2 35 58](https://user-images.githubusercontent.com/56465854/118437368-77656380-b71d-11eb-978f-ff600aa4db57.png)


