
# CSS
>###  가상요소 `:before`, `after`

```
.tab_type1 ul li a:before{
    content:"맛있는";
    width: 40px;
    padding: 3px 6px;
    margin: 0 5px;
    border-radius: 4px;
    background:red;
    text-align:center;
    color: white;
}

```
<img src="https://user-images.githubusercontent.com/30430227/73346127-b1142000-42c8-11ea-8db1-9b214669e73e.jpg">

>### list-style: none
```
리스트 스타일
*{
    padding:0;
    list-style: none;
}
```

>### margin-left: auto
```
왼 쪽 마진을 공유한다 
```
<img src="https://d2.naver.com/content/images/2018/12/helloworld-201811-flex_13.png">

https://d2.naver.com/helloworld/8540176#ch2

>### flex border collapse Tip
```
ul      { border-width: 2px  0   0  2px }
ul > li { border-width:  0  2px 2px  0  }
```
`border 속성 다음에 border-width가 놓어야 한다`

### New
```
line-height : 2em;
background:url('/images/pic_01.jpg');
clip-path: circle(10px at center); //at 50% 20%, px등으로 위치 지정

    <script>
        var section = document.querySelector('section')
        window.addEventListener('scroll',function(){
            var value = window.scrollY
            section.style.clipPath = 'circle(' + value +'px at center)'
        })
    </script>
    
background: 0% 50% / cover; `X, Y 포지션과 가득채움(cover) 속성`

box-sizing : border-box 와 content-box 차이 

```

> http://placeimg.com/640/480/any

<img src="http://placeimg.com/640/480/any">

# Pure CSS Parallax
### [`출처` https://www.hyungjoo.me/parallax-scroll원리](https://www.hyungjoo.me/parallax-scroll-%EC%9B%90%EB%A6%AC/)
> HTML
```
<div id="title" class="slide header">
  <h1>Pure CSS Parallax</h1>
</div>

<div id="slide1" class="slide">
  <div class="title">
    <h1>Slide 1</h1>
    <p>Lorem ipsum dolor sit amet, in velit iudico mandamus sit, persius dolorum in per, postulant mnesarchum cu nam. Malis movet ornatus id vim, feugait detracto est ea, eam eruditi conceptam in. Ne sit explicari interesset. Labores perpetua cum at. Id viris docendi denique vim.</p>
  </div>
</div>

<div id="slide2" class="slide">
  <div class="title">
    <h1>Slide 2</h1>
    <p>Lorem ipsum dolor sit amet, in velit iudico mandamus sit, persius dolorum in per, postulant mnesarchum cu nam. Malis movet ornatus id vim, feugait detracto est ea, eam eruditi conceptam in. Ne sit explicari interesset. Labores perpetua cum at. Id viris docendi denique vim.</p>
  </div>
  <img src="https://lorempixel.com/640/480/abstract/6/">
  <img src="https://lorempixel.com/640/480/abstract/4/"> 
</div>

<div id="slide3" class="slide">
  <div class="title">
    <h1>Slide 3</h1>
    <p>Lorem ipsum dolor sit amet, in velit iudico mandamus sit, persius dolorum in per, postulant mnesarchum cu nam. Malis movet ornatus id vim, feugait detracto est ea, eam eruditi conceptam in. Ne sit explicari interesset. Labores perpetua cum at. Id viris docendi denique vim.</p>
  </div>
</div>

<div id="slide4" class="slide header">
    <h1>The End</h1>
</div>
```

> CSS
```
@import url(https://fonts.googleapis.com/css?family=Nunito);

html {
  height: 100%;
  overflow: hidden;
}

body { 
  margin:0;
  padding:0;
	perspective: 1px;
	transform-style: preserve-3d;
  height: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  font-family: Nunito;
}

h1 {
   font-size: 250%
}

p {
  font-size: 140%;
  line-height: 150%;
  color: #333;
}

.slide {
  position: relative;
  padding: 25vh 10%;
  min-height: 100vh;
  width: 100vw;
  box-sizing: border-box;
  box-shadow: 0 -1px 10px rgba(0, 0, 0, .7);
	transform-style: inherit;
}

img {
  position: absolute;
  top: 50%;
  left: 35%;
  width: 320px;
  height: 240px;
  transform: translateZ(.25px) scale(.75) translateX(-94%) translateY(-100%) rotate(2deg);
  padding: 10px;
  border-radius: 5px;
  background: rgba(240,230,220, .7);
  box-shadow: 0 0 8px rgba(0, 0, 0, .7);
}

img:last-of-type {
  transform: translateZ(.4px) scale(.6) translateX(-104%) translateY(-40%) rotate(-5deg);
}

.slide:before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  box-shadow: 0 0 8px 1px rgba(0, 0, 0, .7);
}

.title {
  width: 50%;
  padding: 5%;
  border-radius: 5px;
  background: rgba(240,230,220, .7);
  box-shadow: 0 0 8px rgba(0, 0, 0, .7);
}

.slide:nth-child(2n) .title {
  margin-left: 0;
  margin-right: auto;
}

.slide:nth-child(2n+1) .title {
  margin-left: auto;
  margin-right: 0;
}

.slide, .slide:before {
  background: 50% 50% / cover;  
}

.header {
  text-align: center;
  font-size: 175%;
  color: #fff;
  text-shadow: 0 2px 2px #000;
}

#title {
  background-image: url("https://placeimg.com/640/480/abstract/6/");
  z-index:2;
}

#title h1 {
 transform: translateZ(.25px) scale(.75);
 transform-origin: 50% 100%;

}

#slide1:before {
  background-image: url("https://placeimg.com/640/480/abstract/4/");
  transform: translateZ(-1px) scale(2);
}

#slide2 {
  background-image: url("https://lorempixel.com/640/480/abstract/3/");
  z-index:2;
}

#slide3:before {
  background-image: url("https://placeimg.com/640/480/abstract/5/");
  transform: translateZ(-1px) scale(2);
}

#slide4 {
  background: #222;
}
```

# Animation 축약
```
   div {
        animation-name: myShorthand;
        animation-duration: 3s;
        animation-timing-function: ease-in-out;
        animation-delay: 1s;
        animation-iteration-count: 3;
        animation-direction: alternate;
    }
```

> div { animation: myShorthand 3s ease-in-out 1s 3 alternate; }

	@keyframes movingPara {
        from { margin-left: 100%; }
        to { margin-left: 0%; }
	
# em
	폰트 사이즈에 비례 ex)font-size: 16 -> 2em : 32
	
> display : grid; 반응형
	grid-template-columns: 폭 폭 폭 폭 폭;
	grid-template-columns: repeat(5, 폭);
	grid-template-columns: repeat(auto-fit, minmax(19rem, 1fr));
	grid-gap: 1rem;
	cursor: pointer;
	
> image Sprite 유니티의 아틀라스같은... 여러 이미지를 한 장에 모아 사용할 수 있다.
	background: url('...');
	width:
	height:
	background-position: -30,-50;//좌측 상단부위가 width, height 시작하는 지점
	
## SCSS 사용
	 vscode live sass 익스텐션 설치(liveserver 도 동시에 설치된다, 좌측 탐색기에서 오른클릭으로 openWithLiveServer실행할 수 있다)
	 하단 상태표시줄에서 Watch Sass 클릭하면 동일명의 css가 자동생성된다.

## hover 시 자식도 변형효과
	transition : transform 0.5s
	
	:hover 자식선택자{...} // .card:hover img{transform: translateX(-10px);
	transform: rotate( 45deg ) // deg, rad, turn

## addEventListener 에 대한 예제 window.addEventListener('mousemove', e=>{...e.target.offsetTop...)// e는 이벤트 타겟 객체
	<div class="check">
    <p>
        <strong>
            <span>click</span>
        </strong>
    </p>
	</div>

	<div id="result">색이 들어가있는 박스들을 하나씩 클릭해주세요</div>
	
	$("body").click(function(event){
    $("#result").html("무엇을 클릭했을 까요? " + event.target.nodeName);
	})
> css
	div.check{padding:10px;border:1px solid #000;margin-bottom:10px;background-color:#ffeed6;}
	p{padding:10px;border:1px solid #000;background-color:#d6dfff;}
	strong{padding:10px;border:1px solid #000;display:block;background-color:#d6ffdb;}
	span{padding:10x;border:1px solid #000;display:block;background-color:#ffddd6;}

## 이어서 메뉴
	<ul class="one">
    <li>메뉴1
        <ul class="two">
            <li>메뉴1-1</li>
            <li>메뉴1-2</li>
            <li>메뉴1-3</li>
        </ul>
    </li>
    <li>메뉴2
        <ul class="two">
            <li>메뉴2-1</li>
            <li>메뉴2-2</li>
            <li>메뉴2-3</li>
        </ul>
    </li>
    <li>메뉴3
        <ul class="two">
            <li>메뉴3-1</li>
            <li>메뉴3-2</li>
            <li>메뉴3-3</li>
        </ul>
    </li>
	</ul>
***
function handler(event){
    // 2단계. handler 의 함수를 작성. handler 함수가 호출이 되면 아래와 같은 행동을 하라.
    var $target = $(event.target);
        // 현재 클릭했던 요소가 뭔 요소인지 변수 $target 에 담는다.
    if($target.is("li")){
        // 만약에 $target 이 담고있었던 요소가 li 가 맞다면!
        $target.children().toggle();
            // 클릭했던 바로 그 li 의 자식 요소(.one li .two) 을 토글 시켜라.
    }
    
  ***
  *{margin:0;padding:0;}
li{list-style-type:none;text-align:center;}
.one{background-color:#ffddd6;padding:10px;*zoom:1;}
.one:after{content:'';display:block;clear:both;}
.one>li{float:left;background-color:#ffffd6;margin-right:10px;cursor:pointer;}
.two{background-color:#d6ffd9;padding:10px;}
.two>li{background-color:#dad6ff;}


  
}

```
$(".one").click(handler).find("ul").hide();
    // 1단계. one 라는 클래스를 가진 요소를 클릭하게 되면 handler 라는 함수를 동작하게 하라
    // .one 가 자식으로 가지고 있는 요소들 중에 ul 을 찾아 숨겨라 <- 브라우저가 시작하마자 동작
 ```

## border-radius 찌그러짐
> border-radius: 3%/10% //가로가 긴 경우 가로 % 값을 작게준다




