# DoIt바닐라스크립트

## Continue 홀수만 출력
```
    <script>
        for(let i=1;i<=10;++i){
            if(i%2==0)continue
            document.write(i+'<br>')
        }
        document.querySelector('section').write('=== End Game ===')
    </script>
```

## while 구구단
![image](https://user-images.githubusercontent.com/30430227/125612130-89e1ec7a-7f3b-4d7c-a1c8-9476bde807aa.png)
```
    <script>
        let i = 1
        while(i<=9){
            document.write(`5 X ${i} = ${i*5} <br>`)
            i+=1
        }
    </script>
```

## for 테이블
![image](https://user-images.githubusercontent.com/30430227/125613342-32a45c14-499e-4329-9390-c43bacde258f.png)
```
    <script>
        let num = 1
        let t = '<table border=1>'

        for(let i=0;i<5;++i){
            t += '<tr>'
                for(let j=0;j<5;++j){
                    t += `<td> ${num} </td>`
                    num++
                }
            t += '</tr>'
        }

        t += '</table>'
        document.write(t)
    </script>
```

## 가위바위보 맞추기?
![image](https://user-images.githubusercontent.com/30430227/125717676-b9b6c5e5-c143-47c6-bedf-dc78bc255aeb.png)
```
    <script>
        document.write('<h1>컴퓨터 가위, 바위, 보 맞추기</h1> ')

        let gamer = prompt('가위, 바위, 보 중 선택하세요','가위')
        let gamerNum

        switch(gamer){
            case '가위':
                gamerNum = 3
                break
            case '바위':
                gamerNum = 4
                break
            case '보':
                gamerNum = 5
                break
            default:
                alert('니주글래!')
                location.reload()
        }

        let comNum = Math.ceil(Math.random()*3) + 2

        document.write(`<img src="../4g${gamerNum}.jpg">`)

        if(gamerNum ==comNum){
            document.write(`<span style='background:yellow;font-size:3em;'>추카</span> `)
            document.write(gamerNum, comNum)
        }else{
            document.write(`<span style='background:red;font-size:3em;'>니주글래</span> `)
            document.write(gamerNum, comNum)
        }

    </script>
```

![image](https://user-images.githubusercontent.com/30430227/125723794-7bfc11f7-8813-49e6-9a99-a0641046a474.png)
```
    <script>
        document.write('<h1>이메일 유효성 검사</h1>')
        const userEmail = prompt("당신의 이메일 주소는?","")
        const arrUrl = ['co.kr','com','net','or.kr']

        let check1 =false
        let check2 = false

        //indexOf 제일 먼저 찾은 문자의 인덱스 번호 반환
        if(userEmail.indexOf('@')>0)check1=true

        arrUrl.forEach(url=>{
            if(userEmail.indexOf(url)>0)check2 = true
        })

        if(check1 && check2){
            document.write(userEmail)
        }else{
            alert('니주글래')
            location.reload()
        }
    </script>

```

## Window 객체
![image](https://user-images.githubusercontent.com/30430227/125729109-6826a766-0876-4d8c-aa87-df68831184a4.png)
```
    <script>
        document.write(navigator.userAgent+'<br>')
        document.write(screen.width)
    </script>
```

## 문자열 메소드
![image](https://user-images.githubusercontent.com/30430227/125731787-d2379175-1993-4929-94e5-aa4703e4209e.png)
```
    <script>
        let phoneNum = prompt('전화번호?',"010-0000-0000")
        //.substr(a,b) 인덱스 a부터 b개 반환
        //.substring(a,b) 인덱스 a부터 인덱스b까지 반환(b가 작을 경우 b~a)
        let result_1 = phoneNum.substr(-4,9)
        let result_2 = phoneNum.replace('010','니주글래')
        //slice 인덱스 a~b 반환
        let result_3 = phoneNum.slice(-4, phoneNum.length)
        //split(문자) 지정한 문자를 기준으로 나누고 배열 반환(b가 작으면 반환 없음)
        let result_4 = phoneNum.split('-')

        document.write(result_1,"****<br>")
        document.write(result_2,'<br>')
        document.write(result_3,'<br>')
        document.write(result_4,)
    </script>
```

## 배경색 바꾸기
![image](https://user-images.githubusercontent.com/30430227/125734675-749b3ee6-2eb6-4ec9-952d-02d06960547a.png)
```
    <section>
        <button onclick='onColor()'>배경색 바꾸기</button>
    </section>
    <script>
        function onColor(){
            const arrColor =['#ff0','orange','tomato','dodgerblue']
            let arrNum = Math.floor(Math.random()*arrColor.length)
            const bodyTag = document.querySelector('body')

            console.log(arrNum)
            bodyTag.style.background=arrColor[arrNum]
        }
    </script>
```

## event.target, 금쪽같은 부모와 자식들
firstElementChild, chldNodes.item(3)/item 에는 태그요소만 있는게 아니구나!
nextElementSibling
![image](https://user-images.githubusercontent.com/30430227/125756044-7e205db3-4663-4b4d-a1d2-7a36add79b8d.png)
```
    <section>
        <p>금쪽같은 내새끼</p>
        <div class="parent">
            <div class="child one"></div>
            <div class="child two" onclick="onParent()"></div>
            <div class="child three"></div>
        </div>
    </section>
```
```
 <style>
     section{
         margin: 0 auto;
         width: 200px;
         height: 150px;
         display: flex;
         flex-direction: column;
         justify-content: left;
         align-items: center;
         border: 1px solid #000;
         background: orange;
     }
     .parent{
         width: 150px;
         height: 80px;
         border: 1px solid #000;
         background: olive;
         display: flex;
         justify-content: space-around;
         align-items: center;
     }
     .child{
         width: 30px;
         height: 50px;
         background: gold;
     }
     

 </style>
```
```
    <script>
        //.childNodes
        // console.log(document.querySelector('.parent').childNodes.item(3))
        document.querySelector('.parent').firstElementChild.style.background='white'
        document.querySelector('.parent').childNodes.item(3).style.cursor='pointer'
        function onParent(){
        //event.target
            event.target.parentNode.style.background = 'black'
            event.target.nextElementSibling.style.background = 'red'
        }
    </script>
```
