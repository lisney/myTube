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
