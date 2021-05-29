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
 
 # GUI 추가
 ```
     <script type="module">
        import * as THREE from './js/three.module.js'
        import {GUI} from './js/dat.gui.module.js'

        const api ={
            dataString:'Brush',
            dataNumber: 0.7,
            dataBoolean:false,
            dataFunction: function(){
                alert(
                    'dataString:'+this.dataString+'\n'+
                    'dataNumber'+ this.dataNumger+'\n'
                )
            },
            dataColor: 0xffff00, //대신 [255,255,255]를 하면 컬러 Text가 바뀐다

        }

        window.onload = function(){
            const gui = new GUI()

            gui.add(api, 'dataString')
            gui.add(api, 'dataString',['Brush','Lisney','Kyoko']) //입력받을 수 있는 문자열을 제한
            gui.add(api,'dataNumber')
            gui.add(api,'dataNumber', 0,10,1)
            gui.add(api,'dataNumber', {A:0, B:10, C:20})  //수치 타입을 항목으로써 선택할려면
            gui.add(api, 'dataBoolean')

            const myData = gui.addFolder('내문서')
            myData.add(api, 'dataFunction')
            myData.addColor(api, 'dataColor')
            myData.open()
            
            const renderer = new THREE.WebGLRenderer()
            renderer.setSize(300,300)
            document.body.appendChild(renderer.domElement)
            
            const scene = new THREE.Scene()
            scene.background = new THREE.Color('#00ff00')

            myData.addColor(new ColorGUIHelper(scene,'background'),'value').onFinishChange(
                v=>{
                    renderer.render(scene, camera) //씬은 다시 랜더링해줘야 색상이 update된다
                }
                )
                
                const camera = new THREE.PerspectiveCamera(75,2,.1,10)
                
                renderer.render(scene, camera)
            
        }

        class ColorGUIHelper{
            constructor(object, prop){
                this.object = object
                this.prop = prop
            }
            get value(){
                return `#${this.object[this.prop].getHexString()}`
            }
            set value(hexString){
                this.object[this.prop].set(hexString)
            }
        }



    </script>
```

그런데 만약, 값의 변경이 dat.gui가 아닌 다른 곳에서 이루어진 다면, listen을 호출해 주면 됩니다.
gui.add(myData, "dataNumber").listen();

dat.gui를 통한 값의 변경이 발생하면,
gui.add(myData, "dataNumber").onChange(
     v=>{
     ...
     })
     
onChange는 값 변경 중의 매 순간 발생하, onFinishChange는 최종적인 값의 변경이 발생할 때 호출되는 이벤트

```
Drag transform multiple Control 추가할것
