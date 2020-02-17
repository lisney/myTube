>## 
<form> <fieldset> <legend>개인정보:</legend>
    <label>이름: <input type="text"></label><br> 
    <label>주소: <input type="text">
    </label><br> <input type="submit"> </fieldset> </form>

출처: https://webdir.tistory.com/318 [WEBDIR]

>## clear both
```
div 블럭은 넓이를 얼마를 주던간에 아래로만 깔린다.//좌우 공간을 다 차지
float 속성을 주면 옆으로 나란히 배열된다.//텍스트의 경우는 블럭을 둘러싸게된다
블럭 줄바꿈(?)을 하는 기능이 clear이다// left clear는 왼쪽에 붙는 것을 제거(안 붙는다)
#slide ul:after{content:"";display:block;clear:both;} //이렇게하면 ul 다음 블럭부터는 줄을 바꾸어 시작한다
```
>## radio 버튼
```
name은 같고 id(채널)은 다르게한다
<input type="radio" name="pos" id="pos1" checked>
<input type="radio" name="pos" id="pos2">
<input type="radio" name="pos" id="pos3">
Label과 연동하기1 -for 사용
<label for="pos1">일번 선택</label>

Lable과 연동하기2 -라벨에 포함
<label><input...>일번 선택</label>

#pos1 :checked ~ ul // 라디오 채널(id) pos1이 선택되었을 때 뒤의 태그 ul을 선택하여 속성 변경//이미지 슬라이드로
```
전체 코드

    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Document</title>
    </head>
    <body>
    <style>
      *{margin:0;padding:0;}
      ul,li{list-style:none;}
      #slide{height:300px;position:relative;overflow:hidden;}
      #slide ul{width:400%;height:100%;transition:1s;}
      #slide ul:after{content:"";display:block;clear:both;}
      #slide li{float:left;width:25%;height:100%;}
      #slide li:nth-child(1){background:#faa;}
      #slide li:nth-child(2){background:#ffa;}
      #slide li:nth-child(3){background:#faF;}
      #slide li:nth-child(4){background:#aaf;}
      #slide input{display:none;}
      #slide label{display:inline-block;vertical-align:middle;width:10px;height:10px;border:2px solid #666;background:#fff;transition:0.3s;border-radius:50%;cursor:pointer;}
      #slide .pos{text-align:center;position:absolute;bottom:10px;left:0;width:100%;text-align:center;}
      #pos1:checked~ul{margin-left:0%;}
      #pos2:checked~ul{margin-left:-100%;}
      #pos3:checked~ul{margin-left:-200%;}
      #pos4:checked~ul{margin-left:-300%;}
      #pos1:checked~.pos>label:nth-child(1){background:#666;}
      #pos2:checked~.pos>label:nth-child(2){background:#666;}
      #pos3:checked~.pos>label:nth-child(3){background:#666;}
      #pos4:checked~.pos>label:nth-child(4){background:#666;}
    </style>
    <div id="slide">
      <input type="radio" name="pos" id="pos1" checked>
      <input type="radio" name="pos" id="pos2">
      <input type="radio" name="pos" id="pos3">
      <input type="radio" name="pos" id="pos4">
      <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <p class="pos">
        <label for="pos1"></label>
        <label for="pos2"></label>
        <label for="pos3"></label>
        <label for="pos4"></label>
      </p>
    </div>
    </body>
    </html>

자동슬라이드

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Document</title>
      <style>
        *{margin:0;padding:0;}
        ul,li{list-style:none;}
        .slide{height:300px;overflow:hidden;}
        .slide ul{width:calc(100% * 4);display:flex;animation:slide 8s infinite;} /* slide를 8초동안 진행하며 무한반복 함 */
        .slide li{width:calc(100% / 4);height:300px;}
        .slide li:nth-child(1){background:#ffa;}
        .slide li:nth-child(2){background:#faa;}
        .slide li:nth-child(3){background:#afa;}
        .slide li:nth-child(4){background:#aaf;}
        @keyframes slide {
          0% {margin-left:0;} /* 0 ~ 10  : 정지 */
          10% {margin-left:0;} /* 10 ~ 25 : 변이 */
          25% {margin-left:-100%;} /* 25 ~ 35 : 정지 */
          35% {margin-left:-100%;} /* 35 ~ 50 : 변이 */
          50% {margin-left:-200%;}
          60% {margin-left:-200%;}
          75% {margin-left:-300%;}
          85% {margin-left:-300%;}
          100% {margin-left:0;}
        }
      </style>
    </head>
    <body>
      <div class="slide">
        <ul>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
      </div>
    </body>
    </html>

Flex를 사용하는 방법

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Document</title>
      <style>
        *{margin:0;padding:0;}
        ul,li{list-style:none;}
        .slide{height:300px;overflow:hidden;position:relative;}
        .slide ul{width:calc(100% * 4);display:flex;transition:1s;}
        .slide li{width:calc(100% / 4);height:300px;}
        .slide li:nth-child(1){background:#ffa;}
        .slide li:nth-child(2){background:#faa;}
        .slide li:nth-child(3){background:#afa;}
        .slide li:nth-child(4){background:#aaf;}
        .slide input{display:none;}
        .slide .bullet{position:absolute;bottom:20px;left:0;right:0;text-align:center;z-index:10;}
        .slide .bullet label{width:10px;height:10px;border-radius:10px;border:2px solid #666;display:inline-block;background:#fff;font-size:0;transition:0.5s;cursor:pointer;}
        /* 슬라이드 조작 */
        #pos1:checked ~ ul{margin-left:0;}
        #pos2:checked ~ ul{margin-left:-100%;}
        #pos3:checked ~ ul{margin-left:-200%;}
        #pos4:checked ~ ul{margin-left:-300%;}
        /* bullet 조작 */
        #pos1:checked ~ .bullet label:nth-child(1),
        #pos2:checked ~ .bullet label:nth-child(2),
        #pos3:checked ~ .bullet label:nth-child(3),
        #pos4:checked ~ .bullet label:nth-child(4){background:#666;}
      </style>
    </head>
    <body>
      <div class="slide">
        <input type="radio" name="pos" id="pos1" checked>
        <input type="radio" name="pos" id="pos2">
        <input type="radio" name="pos" id="pos3">
        <input type="radio" name="pos" id="pos4">
        <ul>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
        <p class="bullet">
          <label for="pos1">1</label>
          <label for="pos2">2</label>
          <label for="pos3">3</label>
          <label for="pos4">4</label>
        </p>
      </div>
    </body>
    </html>
    
   > float:left 대신 요소 나란히 배열하고 화면 센터에 놓는 방법
   ```
   .pagination a {
-    display: block;
+    display: inline-block;
     width: 30px;
     height: 30px;
-    float: left;
     margin-left: 3px;
     background: url(/images/structure/pagination-button.png);
 }
 -대신 + 라인을 사용한다
 ```
