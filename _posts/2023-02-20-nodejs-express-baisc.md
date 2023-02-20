---
layout: post
title:  "[NODEJS]Nodejs 와 Express 로 웹서버를 구축하기"
date:   2023-02-0220 20:00:00 +0900
categories: node.js
---


## Nodejs 와 Express 로 웹서버를 구축하기

### Step1 NPM 프로젝트 시작

작업디렉토리 만들기
~/side-project/static-web-nodejs 를 만들었다.

```shell
    > npm init
```
엔터를 치다보면 package.json 파일이 생성된다.   

<br />

```shell
    > npm install express --save
```

package.json 에 express 가 dependencies 안에 추가된것을 확인할 수 있다.

### Step2 Express 기반 웝서버 구동

app.js 파일을 프로젝트 루트에 생성한다.
그리고 아래 코드를 복붙 하기!
```javascript
// node_modules 에 있는 express 관련 파일을 가져온다.
var express = require('express')

// express 는 함수이므로, 반환값을 변수에 저장한다.
var app = express()

// 3000 포트로 서버 오픈
app.listen(3000, function() {
    console.log("start! express server on port 3000")
})

// 이제 터미널에 node app.js 를 입력해보자.
```

터미널에 node app.js 를 입력하면 app.js 가 실행된다.   
수정사항이 있을떄 마다. 종료하고, 다시 node app.js 를 입력해야하니   
변경사항이 있으면 재실행 해주는 nodemon 을 설치하자

```shell
    > npm install nonemon
```

종료 (control + c) 하고, nodemon 으로 실행시켜보자.   
```shell
  > nodemon app.js
```   
app.js 내부를 수정하고 저장하면 자동으로 재실행 되는것을 확인할 수 있다.   



### Step3 URL Routing 처리  
app.js에 아래 코드를 추가한다.
```javascript
// request 와 response 라는 인자를 줘서 콜백 함수를 만든다.
// localhost:3000 브라우저에 res.send() 내부의 문자열이 띄워진다.
app.get('/', function(req,res) {
    res.send("<h1>hi friend!</h1>")
})
```
브라우져에서 localhost:3000 으로 들어가면   
hi friend! 글자를 확인할 수 있다.   

프로젝트 디렉토리에 public 디렉토리생성하고,   
/public/main.html 을 만들자.   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>main.html</title>
</head>
<body>
    <h1>main page</h1>

    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. 
        Atque, ullam quos! Iste quae, molestiae vitae tenetur, 
        in a aperiam voluptatibus dicta qui doloribus libero suscipit optio delectus voluptas voluptatem impedit!</p>
</body>
</html>

```   


그리고 app.js 에 아래 코드를 추가해준다.
```javascript
// request 와 response 라는 인자를 줘서 콜백 함수를 만든다.
// localhost:3000 브라우저에 res.sendFile() 내부의 파일이 띄워진다.

app.get('/', function(req,res) {
    res.sendFile(__dirname + "/public/main.html")
})

// localhost:3000/main 브라우저에 res.sendFile() 내부의 파일이 띄워진다.
app.get('/main', function(req,res) {
    res.sendFile(__dirname + "/public/main.html")
})
```

__dirname dms 요청하고자 하는 파일의 경로를 단축시켜주는 절대경로 식별자이다.   

### Step4 Static 디렉토리 설정
/public/main.html 의 body를 수정해 보자.   
```html
<body>
    <h1>main page</h1>

    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. 
        Atque, ullam quos! Iste quae, molestiae vitae tenetur, 
        in a aperiam voluptatibus dicta qui doloribus libero suscipit optio delectus voluptas voluptatem impedit!</p>
       
        <script src = "main.js"></script>
</body>
</html>
```   
/public/main.js 파일을 만들고 아래와 같이 작성하자.
```javascript
console.log("main js loaded");
```

app.js 에 아래 코드를 추가한다.
```javascript
// public 디렉토리를 static으로 기억한다.
// public 내부의 파일들을 localhost:3000/파일명 으로 브라우저에서 불러올 수 있다.
app.use(express.static('public'))
```

지금까지 app.js 의 코드
```javascript
// node_modules 에 있는 express 관련 파일을 가져온다.
let express = require('express')

// express 는 함수이므로, 반환값을 변수에 저장한다.
let app = express();


// public 디렉토리를 static으로 기억한다.
// public 내부의 파일들을 localhost:3000/파일명 으로 브라우저에서 불러올 수 있다.
app.use(express.static('public'))

// 3000 포트로 서버 오픈
app.listen(3000, function() {
    console.log("start! express server on port 3000")
})




app.get('/', function(req,res) {
    res.sendFile(__dirname + "/public/main.html")
})

// localhost:3000/main 브라우저에 res.sendFile() 내부의 파일이 띄워진다.
app.get('/main', function(req,res) {
    res.sendFile(__dirname + "/public/main.html")
})
```   

브라우져에서 개발자도구의 network 탭을 보면 main.js 가 성공적으로 로드된 것을 확인할 수 있다.   

이미지도 삽입 할 수 있다.
main.html 에 아래 처럼 img 태그를 넣고,
/public/images/cats.jpeg 파일을 위치시킨다.   
```html
<body>
<h1>main page</h1>

<p>Lorem, ipsum dolor sit amet consectetur adipisicing elit.
    Atque, ullam quos! Iste quae, molestiae vitae tenetur,
    in a aperiam voluptatibus dicta qui doloribus libero suscipit optio delectus voluptas voluptatem impedit!</p>

    <img src="images/cats.jpeg">
    <script src = "main.js"></script>
</body>
```   
브라우져에서 이미지를 확인 할 수 있다.   

