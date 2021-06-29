# 기본(fullScreen)
![image](https://user-images.githubusercontent.com/30430227/123611879-01d0d500-d83d-11eb-9d86-c5c073b33605.png)
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <button id="fullscr" style="position: absolute; bottom:50%">
        Go Fullscreen
    </button>
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import Stats from './stats.module.js'


        let renderer, scene, camera, controls, stats
        let container, mesh, fullscreen
        const clock = new THREE.Clock()

        const fsBtn = document.querySelector('#fullscr')

        
        fsBtn.addEventListener('click', e=>{
            e.preventDefault()
            if(!fullscreen){
                fullscreen = true
                document.documentElement.requestFullscreen()
                fsBtn.textContent = "Exit Fullscreen"
            }else{
                fullscreen = false
                document.exitFullscreen()
                fsBtn.innerHTML ='Go Fullscreen'
            }
        })
        
        init()
        animate()

        function init(){
            renderer = new THREE.WebGLRenderer({antialias: true})
            renderer.setSize(window.innerWidth, window.innerHeight)
            document.body.appendChild(renderer.domElement)

            scene = new THREE.Scene()
            // scene.background = new THREE.Color('aqua')

            camera = new THREE.PerspectiveCamera(50, window.innerWidth/window.innerHeight, .1, 100)
            camera.position.set(0,3,5)
            camera.lookAt(scene.position)
            controls = new OrbitControls(camera, renderer.domElement)

            stats = Stats()
            document.body.appendChild(stats.dom)

            {
                const light = new THREE.PointLight()
                light.position.set(10,25,10)
                scene.add(light)
            }

            //Floor
            const floorGeometry = new THREE.PlaneGeometry(10,10, 10,10)

            const textureLoader = new THREE.TextureLoader()
            textureLoader.load('../checkerboard.jpg',texture=>{
                texture.wrapS = texture.wrapT = THREE.RepeatWrapping
                texture.repeat.set(10,10)
                const floorMaterial = new THREE.MeshBasicMaterial({
                    map:texture,
                    // side: THREE.DoubleSide
                })
                const floor = new THREE.Mesh(floorGeometry, floorMaterial)
                floor.position.y = -.5
                floor.rotation.x = -Math.PI/2
                scene.add(floor)
            })

            const skyGeometry = new THREE.BoxGeometry(100,100,100)
            const skyMaterial = new THREE.MeshBasicMaterial({color:0x9999ff, side:THREE.BackSide})
            const sky = new THREE.Mesh(skyGeometry, skyMaterial)
            scene.add(sky)

            // const sphereGeometry = new THREE


            renderer.render(scene, camera)

        }

        window.addEventListener('resize', onWindowResize, false)
        function onWindowResize(){
            camera.aspect = window.innerWidth/window.innerHeight
            camera.updateProjectionMatrix()
            renderer.setSize(window.innerWidth,window.innerHeight)
            renderer.render(scene, camera)
        }

        function animate(){
            controls.update()
            stats.update()
            renderer.render(scene, camera)
            requestAnimationFrame(animate)
        }

    </script>
</body>
</html>
```

# 팝업창 추가
![image](https://user-images.githubusercontent.com/30430227/123762969-48d2cf00-d8fe-11eb-965d-40aa1e87acac.png)
```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     *{
         margin: 0;
         padding: 0;
         list-style: none;
         text-decoration: none;
         box-sizing: border-box;
     }
     /* 팝업 */
     #popup{
         display: none;
         justify-content: center;
         align-items: center;
         background: rgba(0,0,0,.5);
         position: fixed;
         top: 0;
         right: 0;
         bottom: 0;
         left: 0;
        }
        #popup:target{
            display: flex;
            flex-flow: column;
         animation: open 0.5s;
     }

     @keyframes open{
         from{opacity:0} to {opacity:1}
     }
     
     /* 버튼, 박스 부품들 */
     .opener{
         position: fixed;
         left: 100px;
         display: block;
         width: 50px;
         height: 50px;
         background: radial-gradient(circle at 35% 15%, white 1px, aqua 3%, darkblue 60%, aqua 100%);
         font-size: 2em;
         color: white;
         border-radius: 50%;
         text-align: center;
     }
     #popup .box{
         width: 300px;
         height: 200px;
        padding: 5px;
        background: white;
     }
     #popup .bHead{
        display: flex;
        justify-content: space-between;
        border: 1px solid #000;
        width: 100%;
        padding: 5px;
     }
     #popup .close{
         display: block;
         width: 20px;
         height: 20px;
         border: 1px solid #000;
         text-align: center;
         border-radius: 50%;
         line-height: 20px;
     }

 </style>
</head>
<body>
<a href="#popup" class="opener">?</a>
<div id="popup">
    <div class="box">
        <div class="bHead">
            <span>
                Demo Information
            </span> 
                <a href="#" class="close">X</a>
        </div>
        <div class="bBody">
            This is demo, part of a collection.
        </div>
    </div>
    
</div>

<button id="fsBtn" style="position: absolute; bottom:50%; padding: 5px 20px;">Fullscreen</button>

<script>
    let fullscreen

    const fsBtn = document.querySelector('#fsBtn')
    
    fsBtn.addEventListener('click',e=>{
        e.preventDefault()
        if(!fullscreen){
            fullscreen = true
            document.documentElement.requestFullscreen()
            fsBtn.textContent = 'Exit'
        }else{
            fullscreen = false
            document.exitFullscreen()
            fsBtn.innerHTML = 'Fullscreen'
        }
    })
</script>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import Stats from './stats.module.js'

    let renderer, scene, camera, controls, stats
    let container, mesh

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({antialias: true})
        renderer.setSize(window.innerWidth, window.innerHeight)
        document.body.appendChild(renderer.domElement)

        scene = new THREE.Scene()

        camera = new THREE.PerspectiveCamera(50, window.innerWidth/window.innerHeight,.1, 100)
        camera.position.set(0,3,5)
        camera.lookAt(scene.position)
        controls = new OrbitControls(camera, renderer.domElement)

        {
            const light = new THREE.PointLight()
            light.position.set(10,10,10)
            scene.add(light)
        }

        const floorGeometry = new THREE.PlaneGeometry(10,10,10,10)

        const tLoader = new THREE.TextureLoader() 
        tLoader.load('../checkerboard.jpg', texture=>{
            texture.wrapS = texture.wrapT = THREE.RepeatWrapping
            texture.repeat.set(10,10)
            const floorMaterial = new THREE.MeshBasicMaterial({
                map:texture
            })

            const floor = new THREE.Mesh(floorGeometry, floorMaterial)
            floor.position.y = -1
            floor.rotation.x = -Math.PI/2
            scene.add(floor)
        })

        stats = Stats()
        document.body.appendChild(stats.dom)
    }

    window.addEventListener('resize',onWindowResize, false)

    function onWindowResize(){
        camera.aspect = window.innerWidth/window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    }

    function animate(){
        controls.update()
        stats.update()
        renderer.render(scene, camera)

        requestAnimationFrame(animate)

    }

</script>
</body>
</html>
```

