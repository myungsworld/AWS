# AWS

aws 책 몇페이지만 읽었을 뿐인데도 학교 통신 수업에서 듣던 용어가 와르르..  

### IAM
리소스에 대한 액세스 관리  
역할을 가지게 될 __서비스 주체__ 와 그 서비스 주체에 어떤 역할을 부여할지에 대한 __정책__  

즉, 내가 서비스할 주체에 대한 정책을 설정하는 곳  


### SSH(Secure Shell Protocol)
네트워크 프로토콜 중 하나로 공공 네트워크로 서로 통신할때 보안을 가지고 안전하게 사용하기 위함  

대표적인 예로는 데이터 전송과 원격제어  
데이터 전송에는 원격 저장소인 깃허브(여기^^)에 소스코드를 전송할때 SSH를 활용해 파일 전송  
원격 제어: AWS를 통해 생성한 인스턴스에 접속해 명령을 내리기 위해 SSH로 접속 해야 함  

### AWSForWordPressPlugin
Amazon Polly(TTS) : Text to Speech 문자 그대로 글을 음성으로 변환해주는 API  
서버를 영원히 켜둘수 있다면 좋을텐데 월마다 돈이 나가는걸 원치 않기 때문에 이 사이트는 다시 볼수 없을것이다람쥐 http://13.124.136.253/ 내 첫 서버;;  
DNS보다 저게 더 클래식하고 좋은듯  

### S3 (Simple Storage Service)
http://bucket-bootstrap-blog-exmaple.s3-website.ap-northeast-2.amazonaws.com/  두번째 내 홈페이지 였던것;;  
<img width="1440" alt="스크린샷 2020-10-08 오후 3 33 49" src="https://user-images.githubusercontent.com/56465854/95423427-0508f580-097c-11eb-991c-931f1b1a3a9d.png">

### DynamoDB(NoSQL)
데이터베이스는 빠지면 안되지  
<img width="1440" alt="스크린샷 2020-10-08 오후 4 52 53" src="https://user-images.githubusercontent.com/56465854/95430655-07bd1800-0987-11eb-89d5-9508b7c24ed3.png">

### AWSLambda
SNS 메세지 푸시  
IAM에서 SNS 정책 쓰기로 생성 한 다음 새로 람다 함수를 만들어서 그 정책을 적용 시킨다음 
```javascript
const AWS = require('aws-sdk');
    
exports.handler = (event, context, callback) => {
    const params = {
        Message: event.text,
        PhoneNumber: event.number
    };
    
    const publishTextPromise = new AWS.SNS({ apiVersion: '2010-03-31',region: 'ap-northeast-1'}).publish(params).promise();
    publishTextPromise.then(
        function(data){
            callback(null,"MessageID is "+ data.MEssageId);
        }).catch(
            function(err){
                callback(err);
            });
};
```
코드를 넣고 json 에 보낼 내용과 폰번호를 적으면 
