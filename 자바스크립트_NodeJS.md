## 서버 기초 request.url/ response.write, response.end
```
const http = require('http') // http 모듈을 불러온다

const server = http.createServer((request, response)=>{
    console.log(request.url) // '/xxx' 주소
    console.log(request.method) 
    if(request.url ==='/'){ // 주소가 '/'이면
        response.write('Hello') // 서버의 응답, express에선 response.send('text')하면 된다
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

4. #each 반복

![image](https://user-images.githubusercontent.com/30430227/126609548-a83b2faf-8498-46c1-822a-550068a1fe9d.png)
>> server.js
```
server.get('/contact',(req,res)=>{
    res.status(200).render('index.hbs',{  //{name:   } 객체 전달
        contact: true,
        list:[
            'Danny','Jenny','boy'
        ]
    }) 
})
```
>> index.hbs
```
<h1>반갑</h1>
<ul>
    {{#each list}}
    <li>{{this}}</li>
    {{/each}}
</ul>
```

## REST API 1) GET method
![image](https://user-images.githubusercontent.com/30430227/126629970-a22d1be7-7c01-4152-8615-f95d4bb06843.png)
> /api/user 주소를 입력하면(요청하면) json데이터로 응답한다
```
const express = require('express')

const server = express()

const users =[
    {
    id:'Dan',
    name:'Danny',
    email:'d@dan.com'
    },
    {
    id:'lisney',
    name:'Lee',
    email:'lisney@namver.com'
    }

]

server.get('/api/user',(req, res)=>{
    res.json(users)
})


server.listen(3000,()=>{
    console.log('The server is running')
})
```

## REST POST  body-parser 요청 데이터 파싱 미들웨어
```
const express = require('express')

const bodyParser = require('body-parser') //body-parser

const server = express()

server.use(bodyParser.json()) // JSON 파싱

const users =[
    {
    id:'Dan',
    name:'Danny',
    email:'d@dan.com'
    },
    {
    id:'lisney',
    name:'Lee',
    email:'lisney@namver.com'
    }

]

server.get('/api/user',(req, res)=>{
    res.json(users)
})

server.post('/api/user',(req, res)=>{
    console.log(req.body) // 요청 데이터 표시
    users.push(req.body) // 요청 데이터 users에 추가
    res.json(users)
})

server.listen(3000,()=>{
    console.log('The server is running')
})
```
![image](https://user-images.githubusercontent.com/30430227/126738802-6d986bc2-7ea4-4fea-93bd-8c8ca5649804.png)
> 헤더에 보내는 데이터 타입 JSON

![image](https://user-images.githubusercontent.com/30430227/126738842-11d72ce2-d94b-4e1e-8610-65d6c5c15c51.png)
> 바디에 JSON 문 작성 ("쌍따옴표)


## REST ID를 통해 데이터 불러오기 params(params.id)
```
server.get("/api/user/:id",(req, res)=>{
    const user = users.find(u=>{
        return u.id === req.params.id // 요청 : 파라미터 id 와 같은게 있는지 찾기, 있으면 true 리턴
    })
    if(user){
        res.json(user)
    }else{
        res.status(404).json({errorMessage:"User was not found"})
    }
})
```
![image](https://user-images.githubusercontent.com/30430227/126742307-03f57ab0-0fde-49ba-99f8-e28047460665.png)
> 응답!

## REST  put, delete 데이터 업데이트, 삭제
```
server.put('/api/user/:id',(req, res)=>{
    let foundIndex = users.findIndex(u=>u.id===req.params.id)
    if(foundIndex === -1){
        res.status(404).json({errorMessage:'User was not found'})
    }else{
        users[foundIndex] = {...users[foundIndex], ...req.body} // ... Rest Operator 똑같은 프로퍼티가 있다면 뒤에 쓴걸로 덮어쓰기
        res.json(users[foundIndex])
    }
})
```
> server.put > ... Rest Operator 사용

```
server.delete('/api/user/:id', (req, res)=>{
    let foundIndex = users.findIndex(u=> u.id === req.params.id)
    if(foundIndex === -1){
        res.status(404).json({errorMessage:"User was not found"})
    }else{
        let foundUser = users.splice(foundIndex, 1)
        res.json(foundUser[0])
    }
})
```
> server.delete > splice(접합, 결혼)

## 파일업로드 multer 미들웨어
> npm i multer
```
const multer = require('multer')

const upload = multer({
    storage: multer.diskStorage({
        destination:(req, file, cb)=>{
            cb(null,'uploads/') // cb 콜백함수를 통해 전송된 파일 저장 디렉토리 설정
        },
        filename: (req, file, cb)=>{
            cb(null, file.originalname) // cb 콜백함수를 통해 전송된 파일 이름 설정
        }
    })
})
```
```
server.post('/upload',upload.single('img'), (req, res)=>{
    res.send('업로드 성공!'+req.file)
})
```


## 반복 코드
![image](https://user-images.githubusercontent.com/30430227/126763279-d7efa93a-89df-49da-994f-c6e36c6c8f00.png)
> upload.hbs
```
<form action="upload" method='post' enctype="multipart/form-data">
    <input type="file" name="img">
    <input type="submit">
</form>
```

>layout.hbs
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .active{
            color:white;
            background: #000;
        }
    </style>
</head>
<body>
    <img src="./4g2.jpg" style="width: 200px;" alt="">
    {{>nav}}

    {{{ body }}}
    
</body>
</html>
```

> nav.hbs
```
    <nav>
        <a class="{{#if home}}active{{/if}}" href="/">Home</a>
        <a class="{{#if features}}active{{/if}}" href="/features">Features</a>
        <a class="{{#if contact}}active{{/if}}" href="/contact">Contact</a>
    </nav>
```

```
const express = require('express')
const hbs = require('express-handlebars')
const bodyParser = require('body-parser')
//파일업로드
const multer = require('multer')

const upload = multer({
    storage: multer.diskStorage({
        destination:(req, file, cb)=>{
            cb(null,'uploads/') // cb 콜백함수를 통해 전송된 파일 저장 디렉토리 설정
        },
        filename: (req, file, cb)=>{
            cb(null, file.originalname) // cb 콜백함수를 통해 전송된 파일 이름 설정
        }
    })
})

//서버 생성
const server = express()

//hbs 엔진
server.engine(
    'hbs',
    hbs({
        extname:'hbs',
        defaultLayout:'layout',
        layoutsDir:__dirname+'/views/layouts',
        partialsDir:__dirname+'/views/partials'
    })
)

server.set('view engine', 'hbs')

//미들웨어
server.use(express.static(__dirname+'/statics')) // statics 가 루트경로로 지정된다

server.use('/users', express.static(__dirname+'/statics'))// statics 가상경로 지정하기

server.get('/',(req,res)=>{
    res.status(200).sendFile(__dirname+'/index.html') // __dirname 현재 경로
})

server.get('/login', (req, res)=>{
    res.send('<h1>login please Goo!</h1>')
})

// 파일업로드
server.get('/upload', (req, res)=>{
    res.render('upload.hbs',{contact:true}) // '/views' 폴더, contact 변수 전달
})
server.post('/upload',upload.single('img'), (req, res)=>{
    res.send('업로드 성공!'+req.file)
})

////////////////////
const users =[
    {
    id:'Dan',
    name:'Danny',
    email:'d@dan.com'
    },
    {
    id:'lisney',
    name:'Lee',
    email:'lisney@namver.com'
    }

]
////////GET///////////
server.get('/api/user',(req, res)=>{
    res.json(users)
})

////////POST/////////
server.post('/api/user',(req, res)=>{
    console.log(req.body) // 요청 데이터 표시
    users.push(req.body) // 요청 데이터 users에 추가
    res.json(users)
})

////////PUT/////////////
server.put('/api/user/:id',(req, res)=>{
    let foundIndex = users.findIndex(u=>u.id===req.params.id)
    if(foundIndex === -1){
        res.status(404).json({errorMessage:'User was not found'})
    }else{
        users[foundIndex] = {...users[foundIndex], ...req.body} // ... Rest Operator 똑같은 프로퍼티가 있다면 뒤에 쓴걸로 덮어쓰기
        res.json(users[foundIndex])
    }
})

/////////DELETE/////////////
server.delete('/api/user/:id', (req, res)=>{
    let foundIndex = users.findIndex(u=> u.id === req.params.id)
    if(foundIndex === -1){
        res.status(404).json({errorMessage:"User was not found"})
    }else{
        let foundUser = users.splice(foundIndex, 1) //splice 접합, 결혼
        res.json(foundUser[0])
    }
})


server.listen(3000, ['192.168.0.33'], ()=>{
    console.log('The server is running...')
})
```

```

```

