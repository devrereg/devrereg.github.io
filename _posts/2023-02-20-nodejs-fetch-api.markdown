---
layout: post
title:  "[NODEJS]JSON + Fetch API로 웹서버 통신하기"
date:   2023-02-20 20:00:00 +0900
categories: jekyll update
---

### Step1 POST 요청 처리

POST 방식으로 form 데이터를 서버로 보냈을 떄, 서버에서 처리하는 방법을 알아본다. 

```html

<!-- public/form.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <title>email form</title>
</head>
<body>
    <!-- POST 메소드는 서버로 데이터를 전송한다 -->
    <form action="/email_post" method="post">
        email : <input type = "text" name="email"><br/>
        submit : <input type = "submit">
    </form> 
</body>
</html>
```   
form.html 을 만들고, app.js 에 라우트도 만든 후 브라우져로 접속해 본다.   
http://localhost:3000/form   

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/formHtml.png)];

app.js 파일에 post 라우팅 설정을 해준다.

```javascript
// localhost:3000/email_post 에
// res.send() 내부의 내용을 출력한다.
app.post('/email_post', function(req, res) {
    res.send("post response")
})
```   

form.html 에서 이메일 입력후 submit 을 하면 post response 라고 적힌 화면을 볼 수 있다.   

제출한 데이터를 받아서 처리하기 위해서는 body parser 가 필요하다.

```shell
> npm instal body-parser --save
```   

app.js 에 아래 코드를 추가하자.   

```javascript
// 웹브라우저에서 보낸 데이터를 받아서 처리하는 body-parser를 불러온다.
var bodyParser = require('body-parser')

// 브라우저에서 오는 응답이 json 일수도 있고, 아닐 수도 있으므로 urlencoded() 도 추가한다.
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({extended:true}))

// localhost:3000/email_post 에
// res.send() 내부의 내용을 출력한다.
// 브라우저에서 받은 데이터는 req.body 에 저장된다.
app.post('/email_post', function(req, res) {
    res.send("<h1> welcome! </h1>" + req.body.email)
})
```   
브라우져에서 이메일 입력후 제출하면 이메일이 입력된 화면을 볼 수 있다.


### Step3 View engine(ejs)를 활용한 응답처리   

step1 에서 활용한 방식은 send로 보냈다. 하지만 sendFile 로도 보낼 수있다.   
그런데 HTML 이 복잡해지면 위 방식은 활용하기 어렵다.   
이를 해결하기 위해 미리 html 을 만들어 두고, 이를 통해 응답을 할 수 있다.   
먼저 ejs 노드를 설치한다.   

```shell
> npm install ejs --save
```   
 app.js 에 아래 코드를 추가한다.


```javascript
// ejs 는 별다른 require가 필요하지 않다.
// 뷰 엔진을 ejs 로 세팅한다.
app.set('view engine', 'ejs')
```   

프로젝트 디렉터리에 views 디렉토리를 생성하고 emial.ejs 파일을 만든다.

```html
// email.ejs

<!DOCTYPE html>
<html lang="en">

<head>
    <title>Document</title>
</head>

<body>

    <h1> Welcome !! <%= email %></h1>
    <p> 정말로 반가워요 ^^ </p>
</body>

</html>
```   
<%= 내용 $> 는 대체문자열(email 변수가 바인딩 되는자리)이다.   

app.js 에서 /email_post 의 post route 코드를 바꾼다.   

```javascript
    app.post('/email_post', function(req, res) {
    res.render('email.ejs', {'email' : req.body.email})
})
```   
브라우져에서 확인해보면   
/form
[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/formHtml.png)];

<br />

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/post_email.png)];  

<%= email %> 에 request 에서 보낸 email 값이 바인딩 되는것을 확인할 수 있다.   


### Step3 JSON 과 Fetch API 를 활용한 Ajax 처리  

JSON 과 Fetch API를 이용해 서버의 데이터 송수신을 해보자.   
디렉토리 구조부터 세팅!   
/public/data 폴더를 만든다.   
data 아래에 homeContents.json 파일을 만들고 JSON test data 를 넣는다. 
JSON test data 는 인터넷에 검색해보면 쉽게 구할 수 있다.

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/directory_architect.png)];   

app.js 에 아래와같이 작성해주자.
```javascript
// app.js

let createError = require('http-errors');
let express = require('express');
let path = require('path');
let cookieParser = require('cookie-parser');
let logger = require('morgan');
const fs = require('fs');


let homeContents = {};
let planningEvent = {};

// data 폴더 안에 들어있는 json 파일들을 파싱하여 객체 안에 넣는다.
homeContents = JSON.parse(
  fs.readFileSync(__dirname + "/public/data/homeContents.json", "utf-8")
);
planningEvent = JSON.parse(
  fs.readFileSync(__dirname + "/public/data/planningEvent.json", "utf-8")
);

// localhost:3000/homeContents.json 으로 접근하면 파싱한 데이터가 렌더링된다. 
app.get("/homeContents.json", (req,res,next) => {
    res.json(homeContents);
})

app.get("/planningEvent.json", (req,res,next) => {
  res.json(planningEvent);
})
```   
위 과정에서 필요한 의존성을 설치해 준다.   

localhost:3000/homeContents.json 에 접속해보면 더미데이터인 JSON 을 볼 수 있다.   

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/directory_architect.png)];   

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/home_contents_json.png)];   




homeContent.json 내에서도 원하는 프로퍼티만 꺼내보도록 하자.   
브라우져의 콘솔창에서 실행!   
```javascript
let item = fetch("http://localhost:3000/homeContents.json")	
  .then(response => response.json())						
  .then(json => console.log(json.products[0].title));
```

[![form html image](https://devrereg.github.io/assets/img/nodejs-fetchapi/console_capture.png)];   