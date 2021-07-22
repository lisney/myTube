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

