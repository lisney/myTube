## 서버 기초 request.url/ response.write, response.end
```
const http = require('http') // http 모듈을 불러온다

const server = http.createServer((request, response)=>{
    console.log(request.url) // '/xxx' 주소
    console.log(request.method) 
    if(request.url ==='/'){ // 주소가 '/'이면
        response.write('Hello') // 서버의 응답
        response.end()  // 응답 끝
    }else{
        response.write('hi')
        response.end()
    }
})

server.listen(3000) // 서버에서 요청받기(port: 3000)
```

##  서버에서 html 파일 보내기( fs 모듈)
```
const http = require('http')
const fs = require('fs') // 노드js 파일시스템 모듈

const server = http.createServer((request, response)=>{
    console.log(request.url)
    console.log(request.method)
    fs.readFile('./index.html',null, (err, data)=>{
        response.writeHead(200,{ // 200 상태 코드 양호
            'Content-Type':'text/html'
        })
        response.write(data)
        response.end()
    })
})

server.listen(3000)
```

## nodemon 설치
> npm i -g nodemon
```
실행방법 > nodemon server.js
```

## 편한 서버 express 모듈
1. package.json 생성 // 코드 배포를 편하게
> npm init

2. express 모듈 설치
> npm install express
> node_modules 폴더가 생성되어 여기에 설치가 된다


3. 서버 생성
```
const express = require('express')

const server = express()

server.get('/',(req,res)=>{
    res.status(200).sendFile(__dirname+'/index.html') // __dirname 현재 경로
})

server.listen(3000, ()=>{
    console.log('The server is running on Port 3000') //서버 실행 문구
})
```

4. static 파일 사용(express.static 미들웨어) server.use
```
const express = require('express')

const server = express()

server.use(express.static(__dirname)) // express.static 미들웨어 사용

server.use((req,res,next)=>{ //미들웨어 이어서
    //config
    next()
})

server.get('/',(req,res)=>{
    res.status(200).sendFile(__dirname+'/index.html') // __dirname 현재 경로
})

server.listen(3000, ()=>{
    console.log('The server is running on Port 3000!')
})
```

## html랜더링 Template Engine 'handlebars' /{{}} 컬리브라켓
1. handlebars 설치
> npm i express-handlebars

2. 서버 파일
```
const express = require('express')
const hbs = require('express-handlebars')

const server = express()

server.engine('hbs', hbs({
    extname:'hbs',
    defaultLayout:'layout',
    layoutDir:__dirname+'/views/layouts', // views 템플릿 폴더
    partialsDir:__dirname+'/views/partials'
}))

server.set('view engine', 'hbs') // view engine 세팅

server.use(express.static(__dirname+'/statics')) // express.static 미들웨어 사용

server.use((req,res,next)=>{
    //config
    next()
})

server.get('/',(req,res)=>{
    res.status(200).render('index.hbs') // 템플릿 랜더
})

server.listen(3000, ()=>{
    console.log('The server is running on Port 3000!')
})
```

3. layout.hbs
> views/layouts 폴더
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <img src="./4g2.jpg" style="width: 200px;" alt="">
    {{{ body }}} //이 부분에 index.hbs 내용이 들어간다
</body>
</html>
```

4. index.hbs
> views 폴더
```
<h1>안녕</h1>
```
![image](https://user-images.githubusercontent.com/30430227/126600391-15ab168d-5b2c-4df0-823a-8af006ff2f8d.png)

5. 서버에서 index.hbs 로 객체 보내기
> 서버 내용 변경
```
server.get('/',(req,res)=>{
    res.status(200).render('index.hbs',{  //{name:   } 객체 전달
        name:'DannyTWLC'
    }) 
})
```
> index.hbs 내용 변경
```
<h1>안녕</h1>
<h2>{{name}} 안녕 </h2>
```

### nodemon 변경 감시 확장자 설정 방법
>> nodemon server.js --ext .js,.hbs

## handlebars 사용
![image](https://user-images.githubusercontent.com/30430227/126608100-5d13d6de-1753-408b-acae-fbed45c8d493.png)

1. layout.hbs
```
    <style>
        .active{
            color:white;
            background: #000;
        }
    </style>
</head>
<body>
    <img src="./4g2.jpg" style="width: 200px;" alt="">
    <nav>
        <a class="{{#if home}}active{{/if}}" href="/">Home</a>
        <a class="{{#if features}}active{{/if}}" href="/features">Features</a>
        <a class="{{#if contact}}active{{/if}}" href="/contact">Contact</a>
    </nav>
    {{{ body }}}
```

2. server.js
```
server.get('/',(req,res)=>{
    res.status(200).render('index.hbs',{  //{name:   } 객체 전달
        home: true
    }) 
})
server.get('/features',(req,res)=>{
    res.status(200).render('index.hbs',{  //{name:   } 객체 전달
        features: true
    }) 
})
server.get('/contact',(req,res)=>{
    res.status(200).render('index.hbs',{  //{name:   } 객체 전달
        contact: true
    }) 
})
```

3. partials(layout 분리)
![image](https://user-images.githubusercontent.com/30430227/126608655-cf6d2ae4-a7f9-407e-949a-4d38782d3e69.png)
```
<body>
    <img src="./4g2.jpg" style="width: 200px;" alt="">
    {{>nav}}

    {{{ body }}}
    
</body>
```
