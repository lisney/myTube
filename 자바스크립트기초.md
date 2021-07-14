# 리스트, 객체
![image](https://user-images.githubusercontent.com/30430227/125232590-e7f0c100-e317-11eb-8e6b-e09f81b06829.png)
```
<script>
    const list = document.querySelector('ul')

    let data =['디자인', '데이터', '스터디', '간식']

    let jsBook = {
        title:'자바스크립트입문',
        price:25000,
        stock: 3
    }

    data.forEach(i=>{
        const li = document.createElement('li')
        li.innerHTML=`${i}`
        list.appendChild(li)
    })
    console.log(jsBook.title)
    jsBook.title='파이썬'
    console.log(jsBook.title)
</script>
```

# addEventListener, Form, submit
### 전송을 위한 변수(프로퍼티,name) 'word' 
### 입력한 텍스트가 사라지는 원인은 URL이 바뀌어 '새로고침' 하기때문
![image](https://user-images.githubusercontent.com/30430227/125257363-4d54aa00-e338-11eb-93d5-6cdd18c9f154.png)
```
<body>
    <section>
        <form action="#" id="form">
            <input type="text" name="word">
            <input type="submit" value="검색">
        </form>
    </section>
<script>
    const form = document.querySelector('#form')

    form.addEventListener('submit',e=>{
        document.querySelector('#output').textContent = form.word.value
    })
</script>
```

# onsubmit, form, submit
### 반환값(false, true) 따라 요청여부가 결정, return 값 false 시 '새로고침' 안함
![image](https://user-images.githubusercontent.com/30430227/125261929-b0e0d680-e33c-11eb-9102-09ad13b8a67f.png)
```
    <section>
        <form action="#" id="form">
            <input type="text" name="word">
            <input type="submit" value="검색">
        </form>

        <p id="output">하이</p>
    </section>
<script>
    document.querySelector('form').onsubmit=()=>{
        let search = document.querySelector('form').word.value
        document.querySelector('#output').textContent='검색중'
        return false
    }
</script>
```
# 접속
![image](https://user-images.githubusercontent.com/30430227/125263526-11bcde80-e33e-11eb-8756-302615078679.png)
```
    <section>
        <p>최종 접속 일시: <span id="time"></span> </p>
    </section>
<script>
    const now = new Date()
    const year = now.getFullYear()
    const month = now.getMonth()
    const date = now.getDate()
    const hour = now.getHours()
    const min = now.getMinutes()

    document.querySelector('#time').textContent = `${year}년 ${month}/${date} ${hour} : ${min}`
</script>
```

# zfill(파이썬 메소드) 구현
![image](https://user-images.githubusercontent.com/30430227/125268738-f6a09d80-e342-11eb-9927-dc3ec0e4d5da.png)
```
<script>
    function zfill(num, digit){
        let numString = String(num)
        while(numString.length<digit){
            numString = '0'+ numString
        }
        return numString
    }
    const num = '23'

    console.log(zfill(23,5))
</script>
```

# 소수점 floor, toFixed
![image](https://user-images.githubusercontent.com/30430227/125276272-fe644000-e34a-11eb-8e78-de4b22cbbec5.png)
```
    <section>
        <p>원주율은 <span><script>document.write(Math.PI)</script></span></p>
        <p>버리면 <span><script>document.write(Math.floor(Math.PI))</script></span></p>
        <p>소수점 두 자리는 <span><script>document.write(Math.floor(Math.PI*100)/100)</script></span></p>
        <p>메서드 사용 두 자리는 <span><script>document.write((Math.PI).toFixed(2))</script></span></p>
    </section>
```

# 남은 시간은?
### getTime은 밀리세컨드를 받는다
### getMonth는 0~11을 받는다(0 = 1월)
![image](https://user-images.githubusercontent.com/30430227/125278441-b692e800-e34d-11eb-8ca6-dde03ad40771.png)
```
<script>
    function countDown(due){
        const now = new Date()

        const rest = due.getTime() - now.getTime()
        const sec = Math.floor(rest / 1000 % 60)
        const min = Math.floor(rest / 1000 /60) % 60
        const hours = Math.floor(rest / 1000 / 60/60)%24
        const days = Math.floor(rest /1000/60/60/24)
        const count = [days, hours, min, sec]

        return count
    }

    const goal = new Date()
    goal.setHours(21)
    goal.setMinutes(9)
    goal.setSeconds(9)

    console.log(countDown(goal))

</script>
```

# 남은 시간 카운트다운
![image](https://user-images.githubusercontent.com/30430227/125279934-792f5a00-e34f-11eb-8378-742e3bde0227.png)
```
    <section>
        <p>지금부터<span id="timer"></span> 이내 주문하면 5% 할인!</p>
    </section>
<script>
    function countDown(due){
        const now = new Date()

        const rest = due.getTime() - now.getTime()
        const sec = Math.floor(rest / 1000 % 60)
        const min = Math.floor(rest / 1000 /60) % 60
        const hours = Math.floor(rest / 1000 / 60/60)%24
        const days = Math.floor(rest /1000/60/60/24)
        const count = [days, hours, min, sec]

        return count
    }

    const goal = new Date()
    goal.setHours(21)
    goal.setMinutes(9)
    goal.setSeconds(9)
    
    //다른 방법
    //const goal = new Date(2021, 6, 13,13)
    //월의 경우는 (실재 월 - 1) 해야한다

    const timer = document.querySelector('#timer')
    
    recalc()

    function recalc(){
        let counter = countDown(goal)
        timer.textContent = `${counter[0]}일 ${counter[1]}시간 ${counter[2]}분 ${counter[3]}초`
        setTimeout(recalc, 1000)
    }

</script>
```

# 풀다운 메뉴 select
![image](https://user-images.githubusercontent.com/30430227/125384773-60b55300-e3d4-11eb-9ad9-6c9e0d35ab05.png)
```<body>
    <section>
        <form action="" id="form">
            <select name="select" id="">
                <option value="http://www.naver.com">한국어</option>
                <option value="http://www.google.com">English</option>
                <option value="#">日本語</option>
            </select>
        </form>
    </section>
<script>
    //<html lang="ko">
    const lang = document.querySelector('html').lang
    console.log(lang)
    
    document.querySelector('#form').select.onchange = function(){
        location.href = document.querySelector('#form').select.value
    }
</script>
```

# Cookies
![image](https://user-images.githubusercontent.com/30430227/125396880-205fd000-e3e8-11eb-81ad-a01814c84e7d.png)
```
    <section>
        <p>영화관에 가시나요?</p>
        <form action="thank.html" id="form">
            <input type="radio" name='frequency'>주 1회 이상 <br>
            <input type="radio" name='frequency'>월 1회 이상 <br>
            <input type="radio" name='frequency'>1년 1회 이상 <br>
            <input type="radio" name='frequency'>거의 가지 않는다 <br>
            <input type="submit" value='전송하기' id="submit">
        </form>
        <button id="remove">쿠키 삭제</button>
    </section>
    <script src="./js.cookie.js"></script>
    <script>
        document.querySelector('#form').onsubmit = function(){
            if(Cookies.get('answered')==='Yes'){
                window.alert('이미 응답했습니다. 설문지 응답은 한 번만 가능합니다')
                return false
            }else{
            //변수 'answered'에 값 'Yes' 
                Cookies.set('answered', 'Yes',{expires:7})
            }
        }

        document.querySelector('#remove').onclick = function(){
            Cookies.remove('answered')
        }
    </script>
```
    
# data-XXX, dataset 이미지 바꾸기
![image](https://user-images.githubusercontent.com/30430227/125406378-bb11dc00-e3f3-11eb-87a8-b67ce6b3ffb2.png)
 ```
 <style>
     section img{
         max-width: 100%;
     }
     .center{
         margin: 0 auto;
         width: 50%;
         border: 1px solid #000;
     }
     ul{
         overflow: hidden;
         margin: 0;
         padding: 0;
         list-style: none;
     }
     li{
         float: left;
         margin: 0 1% 0 0;
         width: 80px;
     }
 </style>
</head>
<body>
    <section>
        <div class="center">
            <div>
                <img src="../4g2.jpg" id="img" alt="">
            </div>
            <ul>
                <li><img src="../4g3.jpg" class="thumb" data-image="../4g3.jpg" alt=""></li>
                <li><img src="../4g4.jpg" class="thumb" data-image="../4g4.jpg" alt=""></li>
                <li><img src="../4g5.jpg" class="thumb" data-image="../4g5.jpg" alt=""></li>
            </ul>
        </div>
    </section>
    <script>
        const thumbs = document.querySelectorAll('.thumb')
        for(let i =0; i<thumbs.length; i++){
            thumbs[i].onclick = function(){
                console.log(this.dataset.image)
                document.querySelector('#img').src = this.dataset.image
            }
        }
    </script>
```

# 사진 리스트
![image](https://user-images.githubusercontent.com/30430227/125546171-7219b63e-a616-49cb-a3ee-ec9b2ec5fca4.png)
```
 <style>
     section{
         margin: 0 auto;
         width: 300px;
         border: 1px solid #000;
     }
     section img{
         max-width: 100%;
     }

     .toolBar{
        overflow: hidden;
        text-align: center;
     }
     .nav{
         max-width: 100%;
         display: flex;
         justify-content: space-between;
     }
     #prev{
         width: 100px;
         height: 100px;
         background: url(../4g3.jpg);
         background-size: contain;
     }
     #next{
         width: 100px;
         height: 100px;
         background: url(../4g5.jpg);
         background-size: contain;
     }
     #page{
         width: 100px;
         height: 100px;
         font-size: 2em;
     }

 </style>
</head>
<body>
    <section>
            <div class="imgBox">
                <img src="../4g2.jpg" id="img" alt="">
            </div>
            <div class="toolBar">
                <div class="nav">
                    <div id="prev"></div>
                    <div id="page">0</div>
                    <div id="next"></div>
                </div>
            </div>
    </section>
    <script>
        const images =['../4g2.jpg', '../4g3.jpg', '../4g4.jpg', '../4g5.jpg']
        let current = 0

        function changeImage(num){
            if(current + num >= 0 && current + num < images.length){
                current += num
                document.querySelector('#img').src = images[current]
                pageNum()
            }
        }

        document.querySelector('#prev').onclick = function(){
            changeImage(-1)
        }
        document.querySelector('#next').onclick = function(){
            changeImage(1)
        }
        //페이지 넘버
        function pageNum(){
            document.querySelector('#page').textContent = `${current+1} / ${images.length} `
        }

        pageNum()
    </script>
```
# XMLHttpRequest(Vinilla Script Ajax)
### Ajax는 데이터 이용량 줄이고, 페이지 로딩 시간 줄임
> 순서
(클라이언트)XMXHttpRequest 객체 생성/서버로 보냄 => (서버)해당 정보를 클라이언트에 보냄 => (클라이언트)받은 정보를 특정 영역에 뿌림
### XMLHttpRequest 메소드 : open/요청 타입 결정, send/서버로 요청 보냄(GET), send(string)/버서로 요청 보냄(POST)
>open(method, url, async) //method(GET/POST), url(서버/파일 위치), async(true/false)-동기/비동기
### 응답 프로퍼티
>responseType
### 이벤트
>onload/불러왔을 때
>onreadystatechange/속성값 변할 때, readyState/0:요청초기화되지않음/1:서버연결/2:요청받음/3:요청진행중/4:요청응답완료, status/200:정상/404:페이지없음
### POST 방식을 사용할 때: 데이터베이스 업데이트, 많은 양의 데이터(GET방식은 사이즈 제한), 보안

![image](https://user-images.githubusercontent.com/30430227/125599615-c82082bd-92b1-480a-8596-fb2c424057af.png)

```
    <section>
        <ul class="list">
            <li class="seminar" id="js">
                <h3>JavaScript 스터디</h3>
                <p class="check">공석 상황 확인</p>
            </li>
            <li class="seminar" id="security">
                <h3>보안 대책 강좌</h3>
                <p class="check">공석 상황 확인</p>
            </li>
            <li class="seminar" id="uiux">
                <h3>UI/UX 경쟁 대회</h3>
                <p class="check">공석 상황 확인</p>
            </li>
        </ul>
    </section>
```
```
 <style>
     section{
         margin: 0 auto;
         width: 100%;
     }
     .list{
         display: flex;
         justify-content: space-around;
         overflow: hidden;
         margin: 0;
         padding: 0;
         list-style: none;
         text-align: center;
     }
     .list h2{
         margin: 0 0 2em 0;
     }
     .seminar{
         display: block;
         border: 1px solid #000;
         width: 30%;
         padding: 5px;
     }
     .check{
         cursor: pointer;
     }
     .red{
         background: tomato;
     }
     .green{
         background: green;
     }
 </style>
```
```
    <script>
        const check = document.querySelectorAll('.check')

        check.forEach(chk=>{
            getData()
            chk.onclick = function(){
                if(chk.classList.contains('crowded')){
                    chk.textContent = '위험'
                    chk.classList.add('red')
                }else{
                    chk.textContent = '안전'
                    chk.classList.add('green')
                }
            }
        })

        
        function getData(){
            const xhr = new XMLHttpRequest()
            xhr.open('GET', 'data.json')
            xhr.responseType ='json'
            xhr.onload=()=>{
                let data = xhr.response
                for(let i=0;i<data.length;++i){
                    if(data[i].crowded ==='yes'){
                        document.querySelector(`#${data[i].id}`).querySelector('.check').classList.add('crowded')
                    }
                }
            }
            xhr.send()
        }
    </script>
```
### data.json
```
[
    {"id":"js","crowded":"yes"},
    {"id":"security","crowded":"yes"},
    {"id":"uiux","crowded":"no"}
]
```

# 프로미스 연습
```
function makeRequest(location){
    console.log(`${location} 전달`)
    return new Promise((resolve, reject)=>{
        if(location ==='Google'){
            resolve('구글 전달')
        }else{
            reject('니주글래')
        }
    })
}

function processRequest(response){
    return new Promise((resolve, reject)=>{
        resolve(`${response}를 받았다`)
    })
}

async function doWork(){
    try{
        const response = await makeRequest('Gogle')
        console.log(response)
        const processResponse = await processRequest(response)
        console.log(processResponse)
    }catch(err){
        console.log(err)
    }
}
doWork()
```
