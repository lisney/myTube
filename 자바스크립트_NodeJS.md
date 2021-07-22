## 서버 원리 request.url/ response.write, response.end
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

