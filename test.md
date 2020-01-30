
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
