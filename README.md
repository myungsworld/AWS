# AWS

## VPC

[VPC 구축 참조](https://bluese05.tistory.com/48)   
Internet Gateway : 퍼블릭 서브넷과 통신
NAT Gateway : 프라이빗 서브넷이 인터넷과 통신하기 위한 아웃바운드 인스턴스

## Access CodeCommit repo in mutiple AWS accounts with SSH credentials
```shell
ssh-keygen
cat ~/.ssh/id_rsa.pub
```
값 복사 후 적용할 IAM USER -> Security credentials -> Upload SSH public key   
이후 생성된 SSH key ID 값으로 아래 과정 진행  

**한계정 사용할시**  

```shell
~/.ssh/config

Host git-codecommit.*.amazonaws.com
User Your-SSH-Key-ID, such as APKAEIBAERJR2EXAMPLE
IdentityFile Your-Private-Key-File, such as ~/.ssh/codecommit_rsa or ~/.ssh/id_rsa
```

**두계정 이상일시**  

```shell
~/.ssh/config

Host codecommit-1
    Hostname git-codecommit.us-east-1.amazonaws.com
    User SSH-KEY-ID-1 # This is the SSH Key ID you copied from IAM in Amazon Web Services account 1 (for example, APKAEIBAERJR2EXAMPLE1).
    IdentityFile ~/.ssh/codecommit_rsa # This is the path to the associated public key file, such as id_rsa.  We advise creating CodeCommit specific _rsa files.
 
Host codecommit-2
    Hostname git-codecommit.us-east-1.amazonaws.com
    User SSH-KEY-ID-2 # This is the SSH Key ID you copied from IAM in Amazon Web Services account 2 (for example, APKAEIBAERJR2EXAMPLE2).
    IdentityFile ~/.ssh/codecommit_2_rsa # This is the path to the other associated public key file.  We advise creating CodeCommit specific _rsa files.
```

한 계정은 지역을 구분없이 넣어 주고 두개 이상의 계정일시 리젼을 확실하게 기입해 나눠야 한다  
Host의 값은 편한걸로 넣고 git clone ssh://codecommit-2/vi/repos/YOUR-REPO-NAME   

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
    - [EB 설치 on MacOS](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-install-osx.html) 
        -  brew update
        -  brew install awsebcli
        -  eb --version (check)
    - EB 업데이트
        - pip install --upgrade awsebcli
    - [설정 초기화](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb-cli3-configuration.html)
        - eb init
    - .ebignore
        - .ebignore 가 없고 .gitignore이 있는 경우 .gitignore을 사용
        - .ebignore 가 있으면 .gitignore을 읽지 않음
    - .elasticbeanstalk/config.yml 
        - eb init 으로 생성된 환경설정 파일
        - 프로젝트 폴더대신 아티팩트 배포( 디폴트 설정은 파일전체를 올리고 아래 설정을 주면 .zip으로 올리고 압축 풀어서 실행 )
        - ![스크린샷 2021-05-17 오후 3 09 01](https://user-images.githubusercontent.com/56465854/118439931-dc22bd00-b721-11eb-9452-3adb02cc913b.png)
    - eb deploy 
        - 이전까지 설정된 환경을 토대로 배포 
            - 추가옵션
                - export AWS_EB_PROFILE=user2 ( 지정된 프로파일 ( user2 ) 에서 자격 증명을 읽음 )
                - --staged 소스에 커밋 하지 않는 경우 
            - ex ) AWS_EB_PROFILE=default eb deploy project-dev(환경이름) --staged 
    - [eb config (EnvironmentName)](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/command-options-general.html#command-options-general-elbv2)
        - eb config에 대한 모든 내용

---
- [bastion Host](https://velog.io/@jinny/bastion-Host%EB%A1%9C-private-network%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0)
    - 내부와 외부 네트워크 사이에있는 게이트웨이역할을 하는 호스트로 외부에서 바로 내부에 접근하는 것을 막기위해 사용
