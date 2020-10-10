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
<img width="600" alt="스크린샷 2020-10-08 오후 3 33 49" src="https://user-images.githubusercontent.com/56465854/95423427-0508f580-097c-11eb-991c-931f1b1a3a9d.png">

### DynamoDB(NoSQL)
데이터베이스는 빠지면 안되지  
<img width="600" alt="스크린샷 2020-10-08 오후 4 52 53" src="https://user-images.githubusercontent.com/56465854/95430655-07bd1800-0987-11eb-89d5-9508b7c24ed3.png">

### AWSLambda
__SNS 메세지 푸시__  
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
<img width = "300" src="https://user-images.githubusercontent.com/56465854/95545135-fbdd5e80-0a37-11eb-87c9-18bc15594496.png">

### 상태 코드 

200 : 서버가 요청을 제대로 처리한 경우, 즉 클라이언트 요청을 정상적으로 수행함  
201 : 클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성되는 경우  
301 : 클라이언트가 요청한 리소스에 대한 URI가 변경 되었을떄 사용하는 응답 코드  
400 : 요청이 잘못된 경우  
401 : 권한이 없는 경우  
403 : 서버가 요청을 거부하는 경우  
404 : 서버에서 찾을 수 없는 경우(이 에러 ㄹㅇ 한 서른마흔다섯번은 봄)  
500 : 서버 내부에서 에러가 발생한 경우  

---
### API Gateway 와 Database 
API Gateway와 lambda를 쓰면 원하는 어떤 언어든 작성할수 있는 이점이 있다.  

사용할 람다함수를 만들고 트리거에 API Gateway를 선택한다.  
사용할 API와 보안 방식을 선택해야 하는데 보안 방식에는 3가지가 있다.  
1. AWS IAM : IAM 계정을 생성해 인증키를 만들어 인증하는 방법, 사용량의 제한이 없고 한 번 인증받으면 계속해서 사용가능  
2. 열기 : 별도의 인증 없이 누구나 요청을 보낼 수 있다.  
3. API키로 열기: 사용 가능한 인증키를 발급받아 사용함, IAM 권한을 만들지 않아도 사용가능, 인증키별로 사용량과 접근 가능한 API 설정  

__S3 + AWS 인공지능(Amazon translate) + APIGateway +lambda__

요약   
1. 람다 함수 생성 및 소스코드 작성  
2. 람다 함수에 정책 생성 그리고 정책 추가 __(TranslateFullAccess)__  
3. API 게이트웨이 설정  
4. 버킷 생성 한 다음 파일 업로드 및 웹 사이트 호스팅  
+ 여기서 버킷정책을 설정해주지 않아 에러가 났다.  
문제는 해결했지만 정확한 이유는 모르겠다.  
추가한 코드 => 버킷정책에 모든 권한을 퍼블릭으로 풀어서 한건데 구글링해서 에러만 고친꼴  
정책에 대해선 더 심도있게 공부할 필요가 있음.  
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::translate-apigateway-lambda-dynamodb/*"
        }
    ]
}
```
__람다 함수 소스코드__  

```
var AWS = require('aws-sdk');
AWS.config.update({region: 'us-east-1'});

var translate = new AWS.Translate();

exports.handler = function (event, context, callback){
    
    console.log(JSON.stringify(event.body));
    const response = JSON.parse(event.body)
    
    try {
        const translateParams = {
            SourceLanguageCode: 'ko',
            TargetLanguageCode: 'en',
            Text: response.text
        }
        
        translate.translateText(translateParams, function (err, data){
            if (err) callback (err)
            callback(null,{
                statusCode:200,
                headers: {
                    "Access-Control-Allow-Origin" : "*",
                    "Access-Control-Allow-Credentials" : true
                },
                body:data.TranslatedText
            })
        })
    }catch(e){
        callback(null,{
            statusCode:200,
            body:JSON.stringify(e)
        })
```
