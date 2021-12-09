---
title: '[Error] Naver MMS 이미지 전송하기'
author: Jotang
date: 2021-12-09 17:00:00 +0900
categories: [Error]
tags: [error]
math: true
mermaid: true
image: https://images.velog.io/images/jotang3726/post/23510465-8d85-4dc0-be57-ee1964bd0b5f/ERROR.png
---



프로젝트 중 Naver SMS API를 이용하여, 문자메세지를 보내야 하는 기능을 구현하는 중에 만났던 error를 기록하려고 한다.

아래 링크를 클릭하면 NAVER SMS API를 확인할 수 있다.

[NAVER SMS API](https://api.ncloud-docs.com/docs/ai-application-service-sens-smsv2)

```json
{
    "type":"(SMS | LMS | MMS)",
    "contentType":"(COMM | AD)",
    "countryCode":"string",
    "from":"string",
    "subject":"string",
    "content":"string",
    "messages":[
        {
            "to":"string"
        }
    ],
    "files":[
        {
            "name":"string",
            "body":"string"
        }
    ]
}
```

개별 메세지를 이용하거나, 예약 문자를 이용하지 않았기 때문에 위에 나와있는 JSON의 형태로 API 통신을 진행하려고 했다.
SMS / LMS 두 가지 TYPE은 무난히 전송되는 것은 확인하였고, 마지막으로 MMS를 보낼 때, 이미지 파일을 넣어주는 것이 남았다.

![](https://images.velog.io/images/jotang3726/post/60d4773c-b5b5-4358-b9d6-e71408214342/1.png)
위 첨부된 파일 및 NAVER SMS API 홈페이지에서도 확인 할 수 있지만, jpg 또는 jpeg 이미지를 Base64로 인코딩한 값을 전달만 해주면 되는 것이다.

```javascript
const getBase64 = (file) => {
  var reader = new FileReader();
  reader.readAsDataURL(file);
  reader.onload = function(){
    console.log(reader.result);
  };
  reader.onerror = function (error) {
    console.log(error)
  }
}
```

`input의 type='file'`을 이용하여 파일을 가져왔고, react가 아닌 js와 jquery를 이용 중이기 때문에 change 이벤트를 통해서 file의 정보를 가져와서
getBase64()함수에 전달하여 base64로 인코딩된 데이터를 확인 할 수 있었다.

하지만... 다 준비가 되었다고 생각하고 테스트를 진행하는 순간 `400 Bad request` 가 돌아왔다.

정확히 어떤 데이터를 잘못 전달했는지에 대한 자세한 error를 알 수가 없어 삽질이 시작되게 되었다.

(이하 생략.......)

결국 문제는 이미지를 base64로 인코딩한 값을 전달하는데에 문제가 있었고, 다음번엔 까먹지 않기 위해 기록을 해야겠다 싶어 글을 쓴다.

기본적으로 base64로 인코딩 했을 때, 아래와 같은 형태로 인코딩이 된다. 그리고 브라우저나 img태그를 이용하면 해당 이미지 파일을 확인 할 수 있다.

```text
data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAP....
```


여기서 굉장히 상식적인 부분인데 실수를 했다.

```text
data:image/jpeg;base64,
```
이 부분을 지우고 전달을 했어야 했다.

```text
/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAP....
```
이런식으로....ㅎㅎㅎ

앞으로 이러한 작업을 진행할 경우, 다시는 실수하지 말자.
