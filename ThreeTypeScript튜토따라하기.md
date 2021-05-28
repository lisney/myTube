# 멀티뷰
![image](https://user-images.githubusercontent.com/30430227/119948691-0acb4e00-bfd4-11eb-877f-1740ee7b0302.png)
```


 <style>
     .c{
         position: relative;
         width: 200px;
         height: 200px;
     }
  
 </style>
</head>
<body>
    <canvas id="c1" class="c"></canvas>
    <canvas id="c2" class="c"></canvas>
    <canvas id="c3" class="c"></canvas>
    <canvas id="c4" class="c"></canvas>

    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'

        const scene = new THREE.Scene()

        const camera1 = new THREE.PerspectiveCamera(75,1,0.1,10)
        const camera2 = new THREE.OrthographicCamera(-1,1,1,-1,0.1,10)
        const camera3 = new THREE.OrthographicCamera(-1,1,1,-1,0.1,10)
        const camera4 = new THREE.OrthographicCamera(-1,1,1,-1,0.1,10)

        const canvas1 = document.querySelector('#c1')
        const canvas2 = document.querySelector('#c2')
        const canvas3 = document.querySelector('#c3')
        const canvas4 = document.querySelector('#c4')

        const renderer1 = new THREE.WebGLRenderer({canvas:canvas1})
        renderer1.setSize(200,200)

        const renderer2 = new THREE.WebGLRenderer({canvas:canvas2})
        renderer2.setSize(200,200)
        const renderer3 = new THREE.WebGLRenderer({canvas:canvas3})
        renderer3.setSize(200,200)
        const renderer4 = new THREE.WebGLRenderer({canvas:canvas4})
        renderer4.setSize(200,200)

        const controls = new OrbitControls(camera1, renderer1.domElement)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshBasicMaterial({color:0x00ff00, wireframe:true})

        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        camera1.position.z = 2
        camera2.position.y = 1
        camera2.lookAt(new THREE.Vector3(0,0,0))
        camera3.position.z = 1
        camera4.position.x =1
        camera4.lookAt(new THREE.Vector3(0,0,0))

        function animate(time){
            time *=0.001
            cube.rotation.set(time,time,0) 
            controls.update()
            
            renderer1.render(scene, camera1)
            renderer2.render(scene, camera2)
            renderer3.render(scene, camera3)
            renderer4.render(scene, camera4)

            requestAnimationFrame(animate)
        }

        animate()
        
 ```
 
 # GUI
 ![image](https://user-images.githubusercontent.com/30430227/119974723-f8f7a400-bfef-11eb-8f42-32a64a58757f.png)

 
 ```
     <canvas id="c"></canvas>
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import Stats from './stats.module.js'
        import {GUI} from './dat.gui.module.js'


        const canvas1 = document.querySelector('#c')

        const renderer = new THREE.WebGLRenderer()
        renderer.setSize(window.innerWidth/window.innerHeight)
        document.body.appendChild(renderer.domElement)

        const scene = new THREE.Scene()
        const camera = new THREE.PerspectiveCamera(75,2,.1,10)
        camera.position.set(0,0,2)
        
        const controls = new OrbitControls(camera, renderer.domElement)
        controls.addEventListener('change', render)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshBasicMaterial({color:0x00ff00, wireframe:true})
        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        window.addEventListener('resize', onWindowResize, false)
        function onWindowResize(){
            camera.aspect = window.innerWidth/window.innerHeight
            camera.updateProjectionMatrix()
            renderer.setSize(window.innerWidth, window.innerHeight)
        }
        onWindowResize()

        const stats = Stats()
        // stats.showPanel(2)
        document.body.appendChild(stats.dom)

        const gui = new GUI()
        const cubeFolder = gui.addFolder('cube')
        cubeFolder.add(cube.rotation, 'x', 0,Math.PI*2, 0.01)
        cubeFolder.add(cube.rotation, 'y', 0,Math.PI*2, 0.01)
        cubeFolder.add(cube.rotation, 'z', 0,Math.PI*2, 0.01)
        cubeFolder.open()
        const cameraFolder = gui.addFolder('camera')
        cameraFolder.add(camera.position, 'z', 0,10,0.01)
        cameraFolder.open()
        
        function render(){
            stats.begin()
            renderer.render(scene, camera)
            stats.end()
            requestAnimationFrame(render)
        }
        render()
 ```
 
 # 정리할 것
 ```
 var MyData = function() {
    this.dataString = "Dip2K";
    this.dataNumber = 0.7;
    this.dataBoolean = false;
    this.dataFunction = function() {
        alert(
            "dataString: " + this.dataString + "\n" +
            "dataNumber: " + this.dataNumber + "\n" +
            "dataBoolean: " + this.dataBoolean
        );
    };
};
window.onload = function() {
    var myData = new MyData();
        
    var gui = new dat.GUI();
   
    gui.add(myData, "dataString");
    gui.add(myData, "dataNumber");
    gui.add(myData, "dataBoolean");
    gui.add(myData, "dataFunction");
}
```
```
 dataString 변수에 대해서 입력받을 수 있는 문자열을 Dip2K, James, Anold로 제한하고 싶다면 다음처럼 코드를 작성하면 됩니다.

gui.add(myData, "dataString", ["Dip2K", "James", "Anold"]);
dataNumber에 대해서 0~100 사이의 값만을 입력받으며 0.1 단위로 값의 증가, 감소하고자 한다면 아래처럼 코드를 작성하면 되구요.

gui.add(myData, "dataNumber", 0, 100).step(0.1);
dataNumber가 비록 수치 타입이지만, 항목으로써 선택 받도록 하고 각 항목에 대한 수치값을 지정하는 방식도 가능한데, 아래와 같습니다.

gui.add(myData, "dataNumber", { A: 0, B:50, C:100 });
색상값에 대한 직관적인 입력도 가능합니다. 그 예는 아래와 같습니다.

var MyData = function() {
    this.dataString = "Dip2K";
    this.dataNumber = 50;
    this.dataBoolean = false;
    this.dataColor1 = "#ff0000";
    this.dataColor2 = [ 0, 255, 0 ];
    this.dataColor3 = { h: 350, s: 0.5, v: 0.7 };
    this.dataFunction = function() {
        alert(
            "dataString: " + this.dataString + "\n" +
            "dataNumber: " + this.dataNumber + "\n" +
            "dataBoolean: " + this.dataBoolean + "\n" +
            "dataColor1: " + this.dataColor1 + "\n" +
            "dataColor2: " + this.dataColor2 + "\n" +
            "dataColor3: " + this.dataColor3
        );
    };
};
window.onload = function() {
    var myData = new MyData();
    var gui = new dat.GUI();
    gui.add(myData, "dataString", ["Dip2K", "James", "Anold"]);
    gui.add(myData, "dataNumber", 0, 100).step(0.1);
    gui.add(myData, "dataBoolean");
    gui.addColor(myData, "dataColor1");
    gui.addColor(myData, "dataColor2");
    gui.addColor(myData, "dataColor3");
            
    gui.add(myData, "dataFunction");
}
그런데 만약, 값의 변경이 dat.gui가 아닌 다른 곳에서 이루어진 다면, 변경된 값을 dat.gui에서 반영해 줘야 할 것입니다. 이는 매우 간단합니다. 즉, 아래처럼 listen을 호출해 주면 됩니다. 이 listen 함수는 타이머를 생성하고 이 타이머에서 값의 변경 여부를 검사하여 dat.gui에 반영해주게 됩니다.

gui.add(myData, "dataNumber").listen();
끝으로 dat.gui를 통한 값의 변경이 발생하면, 이에 대한 이벤트가 호출되도록 할 수 있는데, 그 예는 아래와 같습니다.

gui.add(myData, "dataNumber").onChange(
    function(v) {
        //alert(v);
    }).onFinishChange(
    function(v) {
        alert(v);
    });
onChange는 값 변경 중의 매 순간 발생하는 이벤트이고, onFinishChange는 최종적인 값의 변경이 발생할 때 호출되는 이벤트입니다.

이외에도 dat.gui는 설정된 값을 localStorage에 저장할 수 있는 기능도 있다는 것도 알아두시면 유용할듯합니다.

```Drag transform multiple Control 추가할것
