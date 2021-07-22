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
        response.writeHead(200,{
            'Content-Type':'text/html'
        })
        response.write(data)
        response.end()
    })
})

server.listen(3000)
```

## express 모듈
1. package.json 생성
> npm init



3. 
