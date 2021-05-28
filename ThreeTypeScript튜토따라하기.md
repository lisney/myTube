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
 
