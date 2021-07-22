//ejs 랜더 서버

const express = require('express');

const app = express();

const router = require('./router/main')(app);

app.engine('html', require('ejs').renderFile);
//app.engine('hbs',

app.set('views', __dirname);
app.set('view engine', 'ejs');
app.use(express.static(__dirname));

app.listen(3000, ()=>{
    console.log("Server has started on port 3000");
});

//라우터 js
module.exports = function(app){
    app.get('/', (req, res)=>{
        res.render('test1.html');
    });
    app.get('/about',(req, res)=>{
        res.render('test2.html');
    });
}

///////////////////////////////////////////////////////////
노드 입력방법- 명령 프롬프트에서 실행

# npm install readline-sync  //우선 명령프롬프트에서 설치부터하고

var readline = require('readline-sync');

var name = readline.question("What is your name?");

console.log("Hi " + name + ", nice to meet you.");

///////////////////////////////////////////////////////////

실행 시 한글 깨질 때 CMD 명령프롬프터에서 코드 페이지를 바꾼다 chcp

And, change the code page to UTF-8.
chcp 65001

////////////////////////////////////////////////////////////

폴더 내 파일 목록 알아내기

var fs = require("fs");

fs.readdir('./', function(err, flist){
    console.log(flist);
});

///////////////////////////////////////////////////////////
/////////////////////// 3 초 후 실행
function run(){
    console.log("3초 후 실행함");
}

console.log("시작");

setTimeout(run,3000);

console.log('끝');

///////////////////////////////////////////////////////////
///////////////// 모듈 생성 및 불러오기 ///////////////////

// 보내는 js 파일
module.exports = {
    sayHello : 'Hello!',
    name : 'CaptainChain'  
}

// 불러오는 js 파일
const test = require('./test');

console.log(test.name + " 님" + test.sayHello)

====================================================================
== npm

>>버전 확인
npm --version

>>업데이트
npm install npm - g

>>express 설치
npm install express // npm install -g express 글로벌, 시스템 디렉토리에 설치

>>package.json 생성
npm init  //npm init --force 바로 생성한다

	name : 이 프로젝트의 이름
	version : 현재 생성하는 npm 파일의 버전. 이전에 따로 배포하는 등의 작업이 없었고, 처음이라면 그대로 엔터를 누른다(1.0.0)으로 설정
	description : 수행할 작업에 대해 작성한다. (ex : A tool to generate file for d3 library)
	entry point : 
	test command : test script에 대해서 정리하려면 꽤 많은 시간을 할애애야하므로, 여기서는 기본 값으로 세
	git repository : github에서 clone 받은 프로젝트라면, 해당 github repository의 주소가 있을 것이다. 만약 그런 경우가 아니라면
	자신의 저장소를 작성해도된다.
	keywords : 이러한 프로젝트를 찾고자하는 사람들에게 유용하게 쓰인다. "file", "d3"등의 키워드를 적어보자.
	author : 개인이나 팀이름, 또는 회사이름 등 다양하게 작성하면 된다.
	license : 라이센스에 대한 내용을 명시한다. 여기에서는 기본적으로 MIT를 설정해본다.

>>모듈 제거
npm uninstall express

>>모듈 업데이트
npm update express

>>express 서버 생성

var express = require("express");
var app = express();
var server = app.listen(3000, function(){
   console.log("express server has started on port 3000"); 
});

>>라우터 생성  // get 매서드를 사용하여
app.get('/', function(req,res){   // URL '/' 로 호출했을 때 콜백함수 실행
    res.send('Hello World');
});
app.get('/about', function(req, res){   //URL '/about' 로 호출....
    res.send("내 이름은 제이");
});

>>라우터 파일 생성 main.js
module.exports = function(app)
{
     app.get('/',function(req,res){
        res.render('index.html')
     });
     app.get('/about',function(req,res){
        res.render('about.html');
    });
}

>>콘솔 입력받기 readline

const readline=require("readline");
const rl=readline.createInterface({
  input:process.stdin,
  output:process.stdout
});
rl.setPrompt("## ");  //cmd의 " > " 같은 앞에 뜨는 문구를 설정하고,
rl.prompt();   //prompt는 입력을 받고,
rl.on("line",(data)=>{  //on은 특정 이벤트에 대한 처리를 합니다. ( 저기에선, line이라는 이벤트가 입력받은 값과 같이 콜백함수를 작동시킵니다
  console.log(data);
  rl.prompt();
});

//multer 사용 파일 업로드
//////////////test.js////////////
let fs = require('fs');
let express = require('express');
let multer = require('multer');
let upload = multer({dest: './uploads'}); //uploads 폴더를 미리 만들어 두지 않으면 생성하고 저장한다

let app = express();

let router = require('./test2')(app); //모듈의 매개변수를 객체로 하여 라우터.get들을 불러온다
app.use(express.static('./public'));  //정적인 파일 서비스 연결 public 폴더를 사용한다(실재로 public폴더는 표현되지 않고 하위폴더//로 연결 (실재 폴더위치 : 해당 폴더\public\css\4GR.jpg 사용은 localhost:3000/css/4GR.jpg)

app.set('views', __dirname + '/'); //__dirname 현재 위치,  '/'=> html 파일 위치
app.set('view engine', 'ejs'); // EJS로 사용한다 Embedded javaScript
app.engine('html', require('ejs').renderFile); //랜더 엔진을 EJS로 사용한다

app.post('/upload', upload.single('myFile'),function(req,res){ //'myFile'은 html1.html input(type="file")의 name과 같아야 한다
    res.send('uploaded :'+req.file.originalname); //업로드 확인 페이지로, 이 라인을 삭제하면 계속 업로드한다
    console.log(req.file); 
});

let server = app.listen(3000, ()=>{
   console.log("3000 서버 실행"); 
});

////////////test2.js/////////////// 라우터 모듈
module.exports = function(app){
    app.get('/', function(req, res){
       res.render('test1.html'); 
    });
    app.get('/about',function(req,res){
        res.render('test2.html');
    });
}

//////////// test1.html ///////////////////////
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="./css/style.css">
</head>
<body>
   <h1>안녕하세요</h1>
   
   <form method="post" action="/upload" enctype="multipart/form-data">
       <table>
           <tr>
               <td><label for="">file</label></td>
               <td><input type="file" name="myFile"></td>
           </tr>
       </table>
       <input type="submit" value="upload" name="submit" />
       
   </form>
   
</body>
</html>
//////////////////////////////////////////////////////
보안 에러 시
이는 윈도우 파워쉘에서 실행했을 때 발생하는 증상으로, 이 때는 "vue.cmd create"로 사용하면 됩니다.
vue.cmd  .cmd 를 붙여준다
http-server.cmd ./
http://localhost:8080/

//////////////////////////////////////////////////////
Handlebars
npm install express-handlebars
node snippets 익스텐션

///////////////////////////// npm 실행시 에러
openssl config failed: error:02001003:system library:fopen:No such process
=> 시스템> 고급 시스템 설정 > 환경 변수 > 시스템 변수에서 OPENSSL 삭제

///////////////////////// 리액트 앱 생성 시 Function 이 아닌 올드버전 Class로 생성되게하는 방법
create-react-app [Project Name] --scripts-version 2.0.3

vmin : 화면을 기준으로 사이즈를 정함 
:: 50vmin 화면 기준으로 50% 사이즈로 정함


/////  GSAP 

///Emmet 리액트에서 사용하기
설정(Ctrl + ,) > WorkSpace setting > Include Languages :: setting.json 후 아래 복붙!
{
   "emmet.includeLanguages": {
      "javascript": "javascriptreact"
   }
}

//express-handlebars 바인딩 :: 라우터.js 파일

const customer ={
    name:'홍길동',
    birthday:'98645',
    gender:'남자',
    job:'대학생',
    list:['국어', '영어', '수학', '과학']
}


module.exports = function(app){
    app.get('/',(req,res)=>{
        // res.write("Hello")
        res.render('index',{customer})
    })
}

/////////////////////////////////// 배열  숫자만큼 실행한다.  {{#each customer.name}} 도 같은 결과
<h1>안녕하세요!</h1>
    {{customer.name}}
    {{customer.age}}
    {{#customer.list}}
        안녕하세요{{this}}<br/>
    {{/customer.list}}

/////////////////////////////////
