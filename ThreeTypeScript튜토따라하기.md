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
            gui.add(api, 'dataString').options(['Brush','Lisney','Kyoko']) //위와 같다
            
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

그런데 만약, 값의 변경이 dat.gui가 아닌 다른 곳에서 이루어진 다면, listen을 호출해 주면 됩니다.
gui.add(myData, "dataNumber").listen();

dat.gui를 통한 값의 변경이 발생하면,
gui.add(myData, "dataNumber").onChange(
     v=>{
     ...
     })
     
onChange는 값 변경 중의 매 순간 발생하, onFinishChange는 최종적인 값의 변경이 발생할 때 호출되는 이벤트

```

# Drag Control
```
    <script type="module">
        import * as THREE from './js/three.module.js'
        import {DragControls} from './js/DragControls.js'
        import Stats from './js/stats.module.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})

            const scene = new THREE.Scene()
            const axesHelper = new THREE.AxesHelper(5)
            scene.add(axesHelper)
            
            const gridHelper = new THREE.GridHelper(5,20,'red', 'dodgerblue') //size, numbers, axisColor, color
            gridHelper.rotation.set(Math.PI/2,0,0)
            scene.add(gridHelper)

            const camera = new THREE.PerspectiveCamera(50, 2, 0.1, 10)
            camera.position.set(0,0,5)

            {
                const light = new THREE.PointLight()
                light.position.set(10,10,10)
                scene.add(light)
            }

            const geometry = new THREE.BoxGeometry(1,1,1)

            function rand(min,max){
                if(max===undefined){
                    max = min;
                    min =0;
                }
                return min + (max - min)*Math.random()
            }

            function makeInstance(x){
                const material = new THREE.MeshPhongMaterial({
                    color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 90%)`,
                    // color:'white',
                    transparent: true,
                })
                const cube = new THREE.Mesh(geometry, material)
                cube.position.set(x,0,0)

                scene.add(cube)

                return cube
            }

            const cubes =[
                makeInstance(-2),
                makeInstance(0),
                makeInstance(2),
            ]

            const controls = new DragControls(cubes, camera, renderer.domElement)
            
            controls.addEventListener('dragstart',event=>{
                event.object.material.opacity =0.33
            })
            controls.addEventListener('dragend',event=>{
                event.object.material.opacity =1
            })

            const stats = new Stats()
            document.body.appendChild(stats.dom)


            function render(){

                renderer.render(scene, camera)
                requestAnimationFrame(render)
            }
            requestAnimationFrame(render)

        }

        main()
```

# transform/multiple Control
```

        import {TransformControls} from './js/TransformControls.js'

            const tControls = new TransformControls(camera, renderer.domElement)
            tControls.setSize(3)
            tControls.space = 'local' //기본은 world

            tControls.attach(cubes[1])
            scene.add(tControls)
            // click 이벤트와 choose 함수를 사용하여 물체를 선택하게할 수 있을듯
            // 사용가능 event : mousedown, mouseup 등
            
                        window.addEventListener('keydown',event=>{
                switch(event.key){
                    case 'w':
                        tControls.setMode('translate')
                        break
                    case 'e':
                        tControls.setMode('rotate')
                        break
                    case 'r':
                        tControls.setMode('scale')
                        break
                }
            })
            
```
# Raycaster face normal
![image](https://user-images.githubusercontent.com/30430227/120093992-aede0200-c158-11eb-88e8-c320ad768398.png)


```
    <script type="module">
        import * as THREE from './js/three.module.js'
        import {OrbitControls} from './js/OrbitControls.js'
        import {GLTFLoader} from './js/GLTFLoader.js'
        import Stats from './js/Stats.module.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})
            renderer.physicallyCorrectLight = true
            renderer.shadowMap.enabled = true

            const scene = new THREE.Scene()
            scene.background = new THREE.Color('gray')

            const axesHelper = new THREE.AxesHelper(5)
            scene.add(axesHelper)
            const gridHelper = new THREE.GridHelper(5,10,'yellow','dodgerblue')
            scene.add(gridHelper)
            const arrowHelper = new THREE.ArrowHelper(new THREE.Vector3(), new THREE.Vector3(), .25, 0xffff00)
            scene.add(arrowHelper)

            const camera = new THREE.PerspectiveCamera(75, 2,0.1,10)
            camera.position.set(0,0,2)

            {
                const light = new THREE.DirectionalLight(0xffffff, 1)
                light.position.set(-1,2,4)
                // scene.add(light)
            }

            const controls = new OrbitControls(camera, renderer.domElement)

            const coneGeometry = new THREE.ConeGeometry(.05, .2, 8)
            const material = new THREE.MeshNormalMaterial()

            const sceneMeshes = new Array()

            const loader = new GLTFLoader()
            loader.load('./gltf/torus.gltf',gltf=>{
                gltf.scene.traverse(child=>{
                    if(child.isMesh){
                        let m = child
                        m.receiveShadow = true
                        m.castShadow = true
                        m.material.flatShading = true;
                        sceneMeshes.push(m)
                    }
                    if(child.isLight){
                        let l =child
                        l.castShadow=true
                        l.shadow.bias = -.003
                        l.shadow.mapSize.width=2048
                        l.shadow.mapSize.height=2048
                    }
                })
                scene.add(gltf.scene)
            },(xhr)=>{
                console.log((xhr.loaded/xhr.total*100) +'% loaded')
            },error=>{
                console.log(error)
            })

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width, height, false)
                }
                return needResize
            }

            const stats = Stats()
            document.body.appendChild(stats.dom)

            function render(){
                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect = canvas.width/canvas.height
                    camera.updateProjectionMatrix()
                }
                controls.update()
                
                renderer.render(scene, camera)
                stats.update()

                requestAnimationFrame(render)
            }
            requestAnimationFrame(render)

            renderer.domElement.addEventListener('dblclick',onDoubleClick, false)
            renderer.domElement.addEventListener('mousemove', onMouseMove, false)
            
            const raycaster = new THREE.Raycaster()

            let intersects = new Array()

            const domRect = canvas.getBoundingClientRect()

            function onMouseMove(event){
                raycaster.setFromCamera({
                    x:((event.clientX-domRect.x)/renderer.domElement.width)*2-1,
                    y:((event.clientY-domRect.y)/canvas.height)*-2+1
                }, camera)
                intersects = raycaster.intersectObjects(sceneMeshes, false)
                if(intersects.length>0){
                    let n = new THREE.Vector3()
                    n.copy(intersects[0].face.normal)
                    n.transformDirection(intersects[0].object.matrixWorld)
                    arrowHelper.setDirection(n)
                    arrowHelper.position.copy(intersects[0].point)
                }
            }

            function onDoubleClick(event){
                const mouse = {
                    x:((event.clientX-domRect.x)/canvas.width)*2-1,
                    y:((event.clientY-domRect.y)/canvas.height)*-2+1
                }
                raycaster.setFromCamera(mouse, camera)
                const intersects= raycaster.intersectObjects(sceneMeshes, false)
                if(intersects.length>0){
                    let n = new THREE.Vector3()
                    n.copy(intersects[0].face.normal)
                    n.transformDirection(intersects[0].object.matrixWorld)
                    const cube = new THREE.Mesh(coneGeometry, material)
                    cube.lookAt(n)
                    cube.rotateX(Math.PI/2)
                    cube.position.copy(intersects[0].point)
                    cube.position.addScaledVector(n, .1)
                    scene.add(cube)
                    sceneMeshes.push(cube)
                }
            }


        }


        main()
```


# drag(boxHelper)
![image](https://user-images.githubusercontent.com/30430227/120190946-3f4d3d00-c254-11eb-96f1-6c42a549fc4a.png)
```
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import {DragControls} from './DragControls.js'

        import Stats from './stats.module.js'
        import {GLTFLoader} from './GLTFLoader.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})
            renderer.shadowMap.enabled = true

            const scene = new THREE.Scene()
            
            const axesHelper = new THREE.AxesHelper(5)
            scene.add(axesHelper)

            const camera = new THREE.PerspectiveCamera(75, 2, 0.1, 10)
            camera.position.set(0,0,4)

            const orbitControls = new OrbitControls(camera, canvas)
            orbitControls.target.set(0,0,0)

            {
                const light1 = new THREE.PointLight(0xffffff, .9)
                light1.position.set(2.5,2.5,2.5)
                light1.castShadow = true
                scene.add(light1)

                const light2 = new THREE.PointLight(0xffffff, .5)
                light2.position.set(-2.5,2.5,2.5)
                light2.castShadow = true
                scene.add(light2)
            }

            const sceneMeshes = new Array()
            let boxHelper

            const dragControls = new DragControls(sceneMeshes, camera, canvas)
            dragControls.addEventListener('hoveron', event=>{
                boxHelper.visible = true
                orbitControls.enabled = false
            })
            dragControls.addEventListener('hoveroff',event=>{
                boxHelper.visible = false
                orbitControls.enabled= true
            })
            dragControls.addEventListener('drag',event=>{
                event.object.position.y = 0
            })
            dragControls.addEventListener('dragstart', event=>{
                boxHelper.visible = true
                orbitControls.enabled = false
            })
            dragControls.addEventListener('dragend',event=>{
                boxHelper.visible = false
                orbitControls.enabled = true
            })

            const planeGeometry = new THREE.PlaneGeometry(25,25)
            const texture = new THREE.TextureLoader().load('../4g2.jpg')
            const plane = new THREE.Mesh(planeGeometry, new THREE.MeshPhongMaterial({map:texture}))
            plane.rotateX(-Math.PI/2)
            plane.position.y = -1
            plane.receiveShadow = true
            scene.add(plane)

            let mixer
            let modelReady = false
            const gltfLoader = new GLTFLoader()
            let modelGroup
            let modelDragBox

            gltfLoader.load('../gltfs/pyramid.gltf', gltf=>{
                gltf.scene.traverse(child=>{
                    if(child){
                        modelGroup = child
                    }
                    if(child.isMesh){
                        child.castShadow = true
                        child.geometry.computeVertexNormals()
                    }

                    modelDragBox = new THREE.Mesh(new THREE.BoxGeometry(2,2,2), new THREE.MeshBasicMaterial({transparent: true, opacity:0}))
                    modelDragBox.geometry.translate(0,0,0) //// center the geometry
                    scene.add(modelDragBox)
                    sceneMeshes.push(modelDragBox)

                    boxHelper = new THREE.BoxHelper(modelDragBox, 0xffff00)
                    boxHelper.visible = false
                    scene.add(boxHelper)

                    scene.add(gltf.scene)

                    modelReady = true
                },xhr=>{
                    console.log((xhr.loaded/xhr.total *100) + '% loaded')//로딩되고 있는 동안 실행
                },error=>{
                    console.log(error)
                })
            })

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width,height, false)
                }
                return needResize
            }

            const stats = Stats()
            document.body.appendChild(stats.dom)

            const clock = new THREE.Clock()

            function render(){
                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect= canvas.width/canvas.height  //window.innerWidth / window.innerHeight
                    camera.updateProjectionMatrix()
                }

                stats.update()
                orbitControls.update()

                if(modelReady){
                    modelGroup.position.copy(modelDragBox.position)
                    boxHelper.update()
                }

                renderer.render(scene, camera)
                requestAnimationFrame(render)
                
            }
            requestAnimationFrame(render)

        }

        main()
```



# mouse Pick
![image](https://user-images.githubusercontent.com/30430227/120286203-f3a09f00-c2f8-11eb-909d-d58f43247327.png)
```
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import {DragControls} from './DragControls.js'

        import Stats from './stats.module.js'
        import {GLTFLoader} from './GLTFLoader.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})
            renderer.shadowMap.enabled = true
            renderer.outputEncoding = THREE.sRGBEncoding

            const scene = new THREE.Scene()
            {
                const light1 = new THREE.PointLight(0xffffff, 0.7)
                const light2 = new THREE.PointLight(0xffffff, 0.7)
                light1.position.set(2,2,2)
                light2.position.set(-2,2,2)
                light1.castShadow = true
                light2.castShadow = true
                scene.add(light1)
                scene.add(light2)
            }

            const camera = new THREE.PerspectiveCamera(75, 2, .1, 100)
            camera.position.set(5,5,5)

            const controls = new OrbitControls(camera, renderer.domElement)

            const pickObjects = new Array()
            let intersectedObject
            let originalMaterials = {}
            const hightlightedMaterial = new THREE.MeshBasicMaterial({wireframe:true, color:0x00ff00})

            const loader = new GLTFLoader()
            loader.load('../gltfs/items.gltf', gltf=>{
                gltf.scene.traverse(child=>{
                    if(child.isMesh){
                        let m = child
                        switch (m.name){
                            case 'Plane':
                                m.receiveShadow = true
                                break
                            case 'Sphere':
                                m.castShadow = true
                                break
                            default:
                                m.castShadow = true
                                pickObjects.push(m)
                                originalMaterials[m.name] = m.material
                        }
                    }
                })
                scene.add(gltf.scene)
            },xhr=>{
                console.log((xhr.loaded/xhr.total * 100)+'% loaded')
            },error=>{
                console.log(error)
            })

            const raycaster = new THREE.Raycaster()
            let intersects

            document.addEventListener('mousemove', onDocumentMouseMove, false)
            const domRect = canvas.getBoundingClientRect()

            function onDocumentMouseMove(event){
                raycaster.setFromCamera({
                    x: ((event.clientX -domRect.x)/renderer.domElement.clientWidth)*2 -1,
                    y: ((event.clientY - domRect.y)/renderer.domElement.clientHeight)*-2+1
                }, camera)
                intersects = raycaster.intersectObjects(pickObjects, false)

                if(intersects.length>0){
                    intersectedObject = intersects[0].object
                }else{
                    intersectedObject = null
                }
                pickObjects.forEach((o, i)=>{
                    if(intersectedObject && intersectedObject.name == o.name){
                        pickObjects[i].material = hightlightedMaterial
                    }else{
                        pickObjects[i].material = originalMaterials[o.name]
                    }
                })
            }

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width,height, false)
                }
                return needResize
            }

            const stats = Stats()
            document.body.appendChild(stats.dom)

            const clock = new THREE.Clock()

            function render(){
                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect= canvas.width/canvas.height  //window.innerWidth / window.innerHeight
                    camera.updateProjectionMatrix()
                }

                stats.update()
                controls.update()

                renderer.render(scene, camera)
                requestAnimationFrame(render)
                
            }
            requestAnimationFrame(render)

        }

        main()
```

# measure
![image](https://user-images.githubusercontent.com/30430227/121773051-570db500-cbb4-11eb-8e8c-ced94582e617.png)

```
<!DOCTYPE html>
<html>

<head>
    <title>Three.js TypeScript Tutorials by Sean Bradley : https://sbcode.net/threejs</title>
    <meta name="author" content="Sean Bradley">
    <style>
        body {
            overflow: hidden;
            margin: 0px;
        }

        #instructions {
            color: white;
            position: absolute;
            left: 100px;
            top: 10px;
            font-family: monospace;
        }

        .measurementLabel {
            font-family: monospace;
            background-color: black;
            color: white;
        }
        #c{
            display: block;
            width: 600px;
            height: 300px;
            position: absolute;
        }
    </style>
</head>

<body>
    <div id="instructions">Press CTRL + Left Mouse Click to start drawing a line. <br />Continue to hold CTRL and Left
        Mouse Click again to mark the end of the line.</div>

    <canvas id="c"></canvas>

    <script type="module">
        import * as THREE from '/js/three.module.js';
        import { OrbitControls } from '/js/OrbitControls.js';
        import { GLTFLoader } from '/js/GLTFLoader.js';
        import Stats from '/js/stats.module.js';
        import { CSS2DRenderer, CSS2DObject } from '/js/CSS2DRenderer.js';

        const canvas = document.querySelector('#c')

        const scene = new THREE.Scene();

        var light = new THREE.SpotLight();
        light.position.set(12.5, 12.5, 12.5);
        light.castShadow = true;
        light.shadow.mapSize.width = 1024;
        light.shadow.mapSize.height = 1024;
        scene.add(light);
        const camera = new THREE.PerspectiveCamera(50,2, 0.1, 1000);
        camera.position.set(15,15,15)
        const renderer = new THREE.WebGLRenderer({canvas});
        renderer.shadowMap.enabled = true;
        renderer.outputEncoding = THREE.sRGBEncoding;
        
        const labelRenderer = new CSS2DRenderer();
        labelRenderer.setSize(canvas.clientWidth, canvas.clientHeight);
        labelRenderer.domElement.style.position = 'absolute';
        labelRenderer.domElement.style.top = '0px';
        labelRenderer.domElement.style.pointerEvents = 'none';
        document.body.appendChild(labelRenderer.domElement);
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        const pickableObjects = new Array();
        const loader = new GLTFLoader();
        loader.load('gltf/simple.gltf', function (gltf) {
            gltf.scene.traverse(function (child) {
                if (child.isMesh) {
                    let m = child;
                    switch (m.name) {
                        case "Plane":
                            m.receiveShadow = true;
                            break;
                        default:
                            m.castShadow = true;
                    }
                    pickableObjects.push(m);
                }
            });
            scene.add(gltf.scene);
        }, (xhr) => {
            console.log((xhr.loaded / xhr.total * 100) + '% loaded');
        }, (error) => {
            console.log(error);
        });

        let ctrlDown = false;
        let lineId = 0;
        let line;
        let drawingLine = false;
        const measurementLabels = {};
        window.addEventListener('keydown', function (event) {
            if (event.key === "Control") {
                ctrlDown = true;
                controls.enabled = false;
                renderer.domElement.style.cursor = "crosshair";
            }
        });
        window.addEventListener('keyup', function (event) {
            if (event.key === "Control") {
                ctrlDown = false;
                controls.enabled = true;
                renderer.domElement.style.cursor = "pointer";
                if (drawingLine) {
                    //delete the last line because it wasn't committed
                    scene.remove(line);
                    scene.remove(measurementLabels[lineId]);
                    drawingLine = false;
                }
            }
        });
        const raycaster = new THREE.Raycaster();
        let intersects;
        const mouse = new THREE.Vector2();
        renderer.domElement.addEventListener('pointerdown', onClick, false);
        function onClick(event) {
            if (ctrlDown) {
                raycaster.setFromCamera(mouse, camera);
                intersects = raycaster.intersectObjects(pickableObjects, false);
                if (intersects.length > 0) {
                    if (!drawingLine) {
                        //start the line
                        const points = [];
                        points.push(intersects[0].point);
                        points.push(intersects[0].point.clone());
                        const geometry = new THREE.BufferGeometry().setFromPoints(points);
                        line = new THREE.LineSegments(geometry, new THREE.LineBasicMaterial({
                            color: 0xff0000,
                            transparent: true,
                            opacity: 0.75
                            // depthTest: false,
                            // depthWrite: false
                        }));
                        line.frustumCulled = false;
                        scene.add(line);
                        const measurementDiv = document.createElement('div');
                        measurementDiv.className = 'measurementLabel';
                        measurementDiv.innerText = "0.0m";
                        const measurementLabel = new CSS2DObject(measurementDiv);
                        measurementLabel.position.copy(intersects[0].point);
                        measurementLabels[lineId] = measurementLabel;
                        scene.add(measurementLabels[lineId]);
                        drawingLine = true;
                    }
                    else {
                        //finish the line
                        const positions = line.geometry.attributes.position.array;
                        positions[3] = intersects[0].point.x;
                        positions[4] = intersects[0].point.y;
                        positions[5] = intersects[0].point.z;
                        line.geometry.attributes.position.needsUpdate = true;
                        lineId++;
                        drawingLine = false;
                    }
                }
            }
        }
        document.addEventListener('mousemove', onDocumentMouseMove, false);

        const domRect = canvas.getBoundingClientRect()
        function onDocumentMouseMove(event) {
            event.preventDefault();
            mouse.x = ((event.clientX-domRect.x) / canvas.clientWidth) * 2 - 1;
            mouse.y = -((event.clientY-domRect.y) / canvas.clientHeight) * 2 + 1;
            if (drawingLine) {
                raycaster.setFromCamera(mouse, camera);
                intersects = raycaster.intersectObjects(pickableObjects, false);
                if (intersects.length > 0) {
                    const positions = line.geometry.attributes.position.array;
                    const v0 = new THREE.Vector3(positions[0], positions[1], positions[2]);
                    const v1 = new THREE.Vector3(intersects[0].point.x, intersects[0].point.y, intersects[0].point.z);
                    positions[3] = intersects[0].point.x;
                    positions[4] = intersects[0].point.y;
                    positions[5] = intersects[0].point.z;
                    line.geometry.attributes.position.needsUpdate = true;
                    const distance = v0.distanceTo(v1);
                    measurementLabels[lineId].element.innerText = distance.toFixed(2) + "m";
                    measurementLabels[lineId].position.lerpVectors(v0, v1, .5);
                }
            }
        }
        const stats = Stats();
        document.body.appendChild(stats.dom);
        var animate = function () {
            renderer.setSize(canvas.clientWidth, canvas.clientHeight, false)
            requestAnimationFrame(animate);
            controls.update();
            render();
            stats.update();
        };
        function render() {
            labelRenderer.render(scene, camera);
            renderer.render(scene, camera);
        }
        animate();
        window.focus()   
    </script>
</body>

</html>
```

# 블렌더에서 GLTF 보내기
```
> 라이트 Include:Puncual Lights 체크

> Bone Aniamtion
 각각 Track 클립 넣기 >  클립 교차되지 않게 위치 옮기기
 이름 바꾸기 : Animation Data 에서 애니메이션을 선택한 후 이름바꾼다
 시작 프레임 위치로 옮긴 후 Export

```
![image](https://user-images.githubusercontent.com/30430227/120104397-3c3c4900-c18f-11eb-8143-51211c5121ec.png)
[그램] 트랙에 배치한 후 mute 한다(체크해제)


# gltf animation
![image](https://user-images.githubusercontent.com/30430227/120741003-3cda3400-c52f-11eb-8f96-e6df8bb82058.png)

```
			import * as THREE from './three.module.js';

			import Stats from './stats.module.js';
			import { GUI } from './dat.gui.module.js';

			import { GLTFLoader } from './GLTFLoader.js';

			let container, stats, clock, gui, mixer, actions, activeAction, previousAction;
			let camera, scene, renderer, model, face;

			const api = { state: 'actionA' };

			const canvas = document.querySelector('#c')

			init();
			animate();

			function init() {
				renderer = new THREE.WebGLRenderer( {canvas:canvas, antialias: true } );
				renderer.outputEncoding = THREE.sRGBEncoding;
	
				camera = new THREE.PerspectiveCamera( 45, 2, 0.25, 100 );
				camera.position.set( -3, 3, 7 );
				camera.lookAt( new THREE.Vector3( 0, 2, 0 ) );

				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0xe0e0e0 );
				scene.fog = new THREE.Fog( 0xe0e0e0, 20, 100 );

				clock = new THREE.Clock();

				// lights

				const hemiLight = new THREE.HemisphereLight( 0xffffff, 0x444444 );
				hemiLight.position.set( 0, 20, 0 );
				scene.add( hemiLight );

				const dirLight = new THREE.DirectionalLight( 0xffffff );
				dirLight.position.set( 0, 20, 10 );
				scene.add( dirLight );

				// ground

				const mesh = new THREE.Mesh( new THREE.PlaneGeometry( 2000, 2000 ), new THREE.MeshPhongMaterial( { color: 0x999999, depthWrite: false } ) );
				mesh.rotation.x = - Math.PI / 2;
				scene.add( mesh );

				const grid = new THREE.GridHelper( 200, 40, 0x000000, 0x000000 );
				grid.material.opacity = 0.2;
				grid.material.transparent = true;
				scene.add( grid );

				// model

				const loader = new GLTFLoader();
				loader.load( '../gltfs/actions.gltf', function ( gltf ) {

					model = gltf.scene;
					scene.add( model );

					createGUI( model, gltf.animations );

				}, undefined, function ( e ) {

					console.error( e );

				} );


				// stats
				stats = new Stats();
				document.body.appendChild( stats.dom );

			}

			function createGUI( model, animations ) {

				const states = [ 'actionA', 'actionB' ];
				const emotes = [ 'Jump', 'Yes', 'No', 'Wave', 'Punch', 'ThumbsUp' ];

				gui = new GUI();

				mixer = new THREE.AnimationMixer( model );

				actions = {};

				for ( let i = 0; i < animations.length; i ++ ) {

					const clip = animations[ i ];
					const action = mixer.clipAction( clip );
					actions[ clip.name ] = action;

					if ( emotes.indexOf( clip.name ) >= 0 || states.indexOf( clip.name ) >= 4 ) {

						action.clampWhenFinished = true;
						action.loop = THREE.LoopOnce;

					}

				}

				// states

				const statesFolder = gui.addFolder( 'States' );

				const clipCtrl = statesFolder.add( api, 'state' ).options( states );

				clipCtrl.onChange( function () {

					fadeToAction( api.state, 0.5 );

				} );

				statesFolder.open();

				// emotes

				// const emoteFolder = gui.addFolder( 'Emotes' );

				// function createEmoteCallback( name ) {

				// 	api[ name ] = function () {

				// 		fadeToAction( name, 0.2 );

				// 		mixer.addEventListener( 'finished', restoreState );

				// 	};

				// 	emoteFolder.add( api, name );

				// }

				// function restoreState() {

				// 	mixer.removeEventListener( 'finished', restoreState );

				// 	fadeToAction( api.state, 0.2 );

				// }

				// for ( let i = 0; i < emotes.length; i ++ ) {

				// 	createEmoteCallback( emotes[ i ] );

				// }

				// emoteFolder.open();

				// expressions

				face = model.getObjectByName( 'CubeA' );

				const expressions = Object.keys( face.morphTargetDictionary );
				const expressionFolder = gui.addFolder( 'Expressions' );

				for ( let i = 0; i < expressions.length; i ++ ) {

					expressionFolder.add( face.morphTargetInfluences, i, 0, 1, 0.01 ).name( expressions[ i ] );

				}

				activeAction = actions[ 'actionA' ];
				activeAction.play();

				expressionFolder.open();

			}

			function fadeToAction( name, duration ) {

				previousAction = activeAction;
				activeAction = actions[ name ];

				if ( previousAction !== activeAction ) {

					previousAction.fadeOut( duration );

				}

				activeAction
					.reset()
					.setEffectiveTimeScale( 1 )
					.setEffectiveWeight( 1 )
					.fadeIn( duration )
					.play();

			}

			//

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width, height, false)
                }

                return needResize
            }

			function animate() {

				const dt = clock.getDelta();

				if ( mixer ) mixer.update( dt );

                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect = canvas.width/canvas.height
                    camera.updateProjectionMatrix()
                }
                
				renderer.render( scene, camera );
				requestAnimationFrame( animate );

				stats.update();

			}


```

# Multi Controls
![image](https://user-images.githubusercontent.com/30430227/121470318-fdad5680-c9f8-11eb-828e-bd057815ae14.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     #c{
         display: block;
         width: 600px;
         height: 300px;
     }
 </style>
</head>
<body>
<video src="" width="300" height="300" autoplay muted></video>
<canvas id="c"></canvas>

<script type="module">
    const video = document.querySelector('video')

    startVideo()
    function startVideo(){
        navigator.mediaDevices.getUserMedia({video})
        .then(stream=>video.srcObject = stream)
        .catch(err=>console.log(err))
    }

    const ctx = document.createElement('canvas').getContext('2d')
    ctx.canvas.width = 300
    ctx.canvas.height = 300
    document.body.appendChild(ctx.canvas)

    ctx.fillStyle = 'red'
    ctx.fillRect(0,0,300,300)

    const img = new Image()
    img.addEventListener('load',()=>{
        ctx.drawImage(img, 20,20,260,260)
    })
    img.src = '../4g2.jpg'

    
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {DragControls} from './DragControls.js'
    import {TransformControls} from './TransformControls.js'
    import Stats from './stats.module.js'
    
    let renderer, scene, camera, cubes, stats
    const canvas = document.querySelector('#c')
    
    main()
    animate()
    
    function main(){
        
        renderer = new THREE.WebGLRenderer({canvas})
        scene = new THREE.Scene()
        const axesHelper = new THREE.AxesHelper(5)
        scene.add(axesHelper)
        
        camera = new THREE.PerspectiveCamera(75, 2, 0.1, 10)
        camera.position.set(0,0,2)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshNormalMaterial({transparent:true})

        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        const orbitControls = new OrbitControls(camera, canvas)

        const dragControls = new DragControls([cube], camera, canvas)
        dragControls.addEventListener('hoveron',()=>{
            orbitControls.enabled = false
        })
        dragControls.addEventListener('hoveroff',()=>{
            orbitControls.enabled = true
        })
        dragControls.addEventListener('dragstart',(event)=>{
            event.object.material.opacity = 0.33
        })
        dragControls.addEventListener('dragend',(event)=>{
            event.object.material.opacity = 1
        })
        stats = Stats()
        document.body.appendChild(stats.dom)

        const texture = new THREE.TextureLoader().load('../umbrellas.png',()=>{
            const rt = new THREE.WebGLCubeRenderTarget(texture.image.height)
            rt.fromEquirectangularTexture(renderer, texture)
            scene.background = rt.texture
        })

        const transformControls = new TransformControls(camera, canvas)
        transformControls.attach(cube)
        transformControls.setMode('rotate')
        transformControls.size =1
        scene.add(transformControls)

        transformControls.addEventListener('dragging-changed',(event)=>{
            orbitControls.enabled = !event.value
            dragControls.enabled = !event.value
        })

        window.addEventListener('keydown', (event)=>{
            switch(event.key){
                case 'w':
                    transformControls.setMode('translate')
                    break

                case 'e':
                    transformControls.setMode('rotate')
                    break

                case 'r':
                    transformControls.setMode('scale')
                    break
            }
        })
        
    }

    
    function animate(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.width/canvas.height
            camera.updateProjectionMatrix()
        }
        stats.update()
        renderer.render(scene, camera)

        requestAnimationFrame(animate)
    }

    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!==canvas.width||height!==canvas.height

        if(needResize){
            renderer.setSize(width, height, false)
        }

        return needResize
    }
    
</script>
</body>
</html>

```


# outline pass

