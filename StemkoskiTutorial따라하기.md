# 기본(fullScreen) - 'f' 키 누름('esc'키 )
![image](https://user-images.githubusercontent.com/30430227/123611879-01d0d500-d83d-11eb-9d86-c5c073b33605.png)
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
     }
 </style>
</head>
<body>
<button id="fsBtn" style="position: absolute; top: 50%;">크게</button>

<script>
    // let fullscreen = false  
    const fsBtn = document.querySelector('#fsBtn')
    window.addEventListener('keyup', event=>{
        // 풀스크린 상태는 document.fullscreenElement 로 확인한다
        if(!document.fullscreenElement && event.key==='f'){
            fsBtn.innerHTML = '작게'
            document.documentElement.requestFullscreen()
            //keyCode==27 혹은 key==='Escape'.. 풀스크린 상태에선 안먹힌다^^
        }else if(document.fullscreenElement && event.key==='f'){
            document.exitFullscreen()
            fsBtn.innerHTML = '크게'
        }
    })
    // esc를 사용하여 전체 화면을 종료 할 때 Chrome은 
    // 주요 이벤트를 발생하지(키 이벤트등) 않습니다. 
    // 그러나 fullscreenchange 이벤트가 발생합니다.
    // window.addEventListener('fullscreenchange',()=>{
    //     if(document.fullscreenElement){
    //         fullscreen = false
    //         fsBtn.innerHTML = '크게'
    //     }
    // }, false)
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
        scene.background = new THREE.Color('aqua')
        scene.fog = new THREE.Fog('aqua', 3,7)

        camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, .1, 100)
        camera.position.set(0,2,4)
        controls = new OrbitControls(camera, renderer.domElement)

        const floorGeometry = new THREE.PlaneGeometry(10,10,10,10)

        const txtLoader = new THREE.TextureLoader() 
        txtLoader.load('../checkerboard.jpg', texture=>{
            texture.wrapS = texture.wrapT = THREE.RepeatWrapping
            texture.repeat.set(10,10)
            const floorMaterial = new THREE.MeshBasicMaterial({
                map: texture,
                side: THREE.DoubleSide
            })
            const floor = new THREE.Mesh(floorGeometry, floorMaterial)
            floor.position.y = -1
            floor.rotation.x = -Math.PI/2
            scene.add(floor)
        })

        stats = Stats()
        document.body.appendChild(stats.dom)
    }

    function animate(){
        stats.update()
        controls.update()
        renderer.render(scene, camera)

        requestAnimationFrame(animate)
    }

    window.addEventListener('resize', onWindowResize, false)

    function onWindowResize(){
        camera.aspect = window.innerWidth/window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    }
</script>
</body>
</html>
```

# 팝업창 추가 - fullscreen 버튼 누름
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

# GUI
![image](https://user-images.githubusercontent.com/30430227/124269594-087d8600-db76-11eb-83e3-8938e6bfdaaa.png)
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
     }
 </style>
</head>
<body>
<button id="fsBtn" style="position: absolute; top: 50%;">크게</button>

<script>
    // let fullscreen = false  
    const fsBtn = document.querySelector('#fsBtn')
    window.addEventListener('keyup', event=>{
        // 풀스크린 상태는 document.fullscreenElement 로 확인한다
        if(!document.fullscreenElement && event.key==='f'){
            fsBtn.innerHTML = '작게'
            document.documentElement.requestFullscreen()
            //keyCode==27 혹은 key==='Escape'.. 풀스크린 상태에선 안먹힌다^^
        }else if(document.fullscreenElement && event.key==='f'){
            document.exitFullscreen()
            fsBtn.innerHTML = '크게'
        }
    })
    // esc를 사용하여 전체 화면을 종료 할 때 Chrome은 
    // 주요 이벤트를 발생하지(키 이벤트등) 않습니다. 
    // 그러나 fullscreenchange 이벤트가 발생합니다.
    // window.addEventListener('fullscreenchange',()=>{
    //     if(document.fullscreenElement){
    //         fullscreen = false
    //         fsBtn.innerHTML = '크게'
    //     }
    // }, false)
</script>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {GUI} from './dat.gui.module.js'
    import Stats from './stats.module.js'
    import {SceneUtils} from './SceneUtils.js'

    let renderer, scene, camera, controls, gui, stats
    let api, sphere

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({antialias: true})
        renderer.setSize(window.innerWidth, window.innerHeight)
        document.body.appendChild(renderer.domElement)

        scene = new THREE.Scene()
        scene.background = new THREE.Color('aqua')
        scene.fog = new THREE.Fog('aqua', 5,9)

        camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, .1, 100)
        camera.position.set(0,2,4)
        controls = new OrbitControls(camera, renderer.domElement)

        const floorGeometry = new THREE.PlaneGeometry(10,10,10,10)

        const txtLoader = new THREE.TextureLoader() 
        txtLoader.load('../checkerboard.jpg', texture=>{
            texture.wrapS = texture.wrapT = THREE.RepeatWrapping
            texture.repeat.set(10,10)
            const floorMaterial = new THREE.MeshBasicMaterial({
                map: texture,
                side: THREE.DoubleSide
            })
            const floor = new THREE.Mesh(floorGeometry, floorMaterial)
            floor.position.y = -1
            floor.rotation.x = -Math.PI/2
            scene.add(floor)
        })

        const sphereGeometry = new THREE.SphereGeometry(1,32,16)
        const sphereMaterial = new THREE.MeshPhongMaterial({
            color:0xff0000, transparent:true, opacity:1
        })
        sphere = new THREE.Mesh(sphereGeometry, sphereMaterial)
        scene.add(sphere)

        {
            const light = new THREE.PointLight(0xffffff)
            light.position.set(1,1,1)
            scene.add(light)
            const lightbulbGeometry = new THREE.SphereGeometry(.1,8,4)
            const lightbulbMaterial = new THREE.MeshBasicMaterial({
                color:'white', transparent:true, opacity:0.8, blending:THREE.AdditiveBlending
            })
        // 멀티 머티리얼
            const wireMaterial = new THREE.MeshBasicMaterial({color:0x000000, wireframe:true})
            const materials =[lightbulbMaterial, wireMaterial]
            const lightbulb = new SceneUtils.createMultiMaterialObject(lightbulbGeometry,materials)
            lightbulb.position.copy(light.position)
            scene.add(lightbulb)

            const aLight = new THREE.AmbientLight(0x333333)
            scene.add(aLight)
        }

        // GUI //

        gui = new GUI()

        api ={
            x:0, y:0, z:0,
            color: '#ff0000',
            colorE: '#000033',
            colorS: '#ffff00',
            shininess:30,
            opacity:1,
            visible:true,
            material:'Phong',
            reset: function(){resetSphere()}
        }

        const folder1 = gui.addFolder('Position')
        const sphereX = folder1.add(api, 'x').min(-1).max(2).step(0.1).listen()
        sphereX.onChange(value=>{
            sphere.position.x = value
        })
        folder1.open()

        const sphereColor = gui.addColor(api,'color').name('Color').listen()
        sphereColor.onChange(value=>{
            sphere.material.color.setHex(value.replace('#','0x'))
        })
        const sphereColorE = gui.addColor(api,'colorE').name('Emissive').listen()
        sphereColorE.onChange(value=>{
            sphere.material.emissive.setHex(value.replace('#','0x'))
        })
        const sphereColorS = gui.addColor(api,'colorS').name('Specular').listen()
        sphereColorS.onChange(value=>{
            sphere.material.specular.setHex(value.replace('#','0x'))
        })
        const sphereShininess = gui.add(api,'shininess',0,60,1).name('Shininess').listen()
        sphereShininess.onChange(value=>{
            sphere.material.shininess = value
        })
        const sphereMat = gui.add(api,'material').options(['Basic','Lambert','Phong','Wireframe']).name('Material Type').listen()
        sphereMat.onChange(value=>{
            updateSphere()
        })
        gui.add(api, 'reset').name("Reset")

        stats = Stats()
        document.body.appendChild(stats.dom)
    }

    function updateSphere()
{
	var value = api.material;
	var newMaterial;
	if (value == "Basic")
		newMaterial = new THREE.MeshBasicMaterial( { color: 0x000000 } );
	else if (value == "Lambert")
		newMaterial = new THREE.MeshLambertMaterial( { color: 0x000000 } );
	else if (value == "Phong")
		newMaterial = new THREE.MeshPhongMaterial( { color: 0x000000 } );
	else // (value == "Wireframe")
		newMaterial = new THREE.MeshBasicMaterial( { wireframe: true } );
	sphere.material = newMaterial;
	
	sphere.position.x = api.x;
	sphere.position.y = api.y;
	sphere.position.z = api.z;
	sphere.material.color.setHex( api.color.replace("#", "0x") );
    if (sphere.material.emissive)
		sphere.material.emissive.setHex( api.colorE.replace("#", "0x") ); 
	if (sphere.material.specular)
		sphere.material.specular.setHex( api.colorS.replace("#", "0x") ); 
    if (sphere.material.shininess)
		sphere.material.shininess = api.shininess;
	sphere.material.opacity = api.opacity;  
	sphere.material.transparent = true;

}

function resetSphere()
{
	api.x = 0;
	api.y = 0;
	api.z = 0;
	api.color = "#ff0000";
	api.colorA = "#000000";
	api.colorE = "#000033";
	api.colorS = "#ffff00";
    api.shininess = 30;
	api.opacity = 1;
	api.visible = true;
	api.material = "Phong";
	updateSphere();
}

    function animate(){
        stats.update()
        controls.update()
        renderer.render(scene, camera)

        requestAnimationFrame(animate)
    }

    window.addEventListener('resize', onWindowResize, false)

    function onWindowResize(){
        camera.aspect = window.innerWidth/window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    }
</script>
</body>
</html>
```

# 점선
![image](https://user-images.githubusercontent.com/30430227/124447726-9ea2ed80-ddbc-11eb-98f5-d11037c0a6f5.png)
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
     }
 </style>
</head>
<body>
    <button id="fsBtn" style="position: absolute; top: 50%;">크게</button>

    <script>
        const fsBtn = document.querySelector('#fsBtn')
        fsBtn.addEventListener('click', event=>{
            if(!document.fullscreenElement){
                fsBtn.innerHTML = '작게'
                document.documentElement.requestFullscreen()
            }else if(document.fullscreenElement){
                document.exitFullscreen()
                fsBtn.textContent = '크게'
            }
        })
    </script>

    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import Stats from './stats.module.js'

        let renderer, scene, camera, controls, stats
        let line

        init()
        animate()

        function init(){
            renderer = new THREE.WebGLRenderer({antialias:true})
            renderer.setSize(window.innerWidth, window.innerHeight)
            document.body.appendChild(renderer.domElement)

            scene = new THREE.Scene()
            scene.background = new THREE.Color('aqua')


            camera = new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight, .1,100)
            camera.position.set(0,0,2)
            camera.lookAt(scene.position)
            controls = new OrbitControls(camera, renderer.domElement)

            const points=[new THREE.Vector3(0,-1,0), new THREE.Vector3(0,1,0)]

            // const geometry = new THREE.BufferGeometry().setFromPoints(points)
            const geometry = new THREE.BufferGeometry().setFromPoints(points)
            const material = new THREE.LineDashedMaterial({
                color:0xff0000, dashSize:.05, gapSize:.1, scale:1, linewidth:1
            })
            const line = new THREE.Line(geometry, material)
            line.computeLineDistances()
            scene.add(line)
            
            stats = Stats()
            document.body.appendChild(stats.dom)
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

# 캔버스 텍스트
![image](https://user-images.githubusercontent.com/30430227/124466934-234c3680-ddd2-11eb-9a25-01619a2000f3.png)

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
     }
 </style>
</head>
<body>
    <button id="fsBtn" style="position: absolute; top: 50%;">크게</button>

    <script>
        const fsBtn = document.querySelector('#fsBtn')
        fsBtn.addEventListener('click', event=>{
            if(!document.fullscreenElement){
                fsBtn.innerHTML = '작게'
                document.documentElement.requestFullscreen()
            }else if(document.fullscreenElement){
                document.exitFullscreen()
                fsBtn.textContent = '크게'
            }
        })
    </script>

    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import Stats from './stats.module.js'

        let renderer, scene, camera, controls, stats, texture
        let line

        init()
        animate()

        function init(){
            renderer = new THREE.WebGLRenderer({antialias:true})
            renderer.setSize(window.innerWidth, window.innerHeight)
            document.body.appendChild(renderer.domElement)

            scene = new THREE.Scene()
            scene.background = new THREE.Color('skyblue')


            camera = new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight, .1,100)
            camera.position.set(0,0,2)
            camera.lookAt(scene.position)
            controls = new OrbitControls(camera, renderer.domElement)

            const floorGeometry = new THREE.PlaneGeometry(10,10,2,2)

            const txtLoader = new THREE.TextureLoader()
            txtLoader.load('../checkerboard.jpg', texture=>{
                texture.wrapS = texture.wrapT = THREE.RepeatWrapping
                texture.repeat.set(10,10)
                const floorMaterial = new THREE.MeshBasicMaterial({
                    map:texture,side:THREE.DoubleSide
                })
                const floor = new THREE.Mesh(floorGeometry, floorMaterial)
                floor.position.y = -1
                floor.rotation.x = -Math.PI/2
                scene.add(floor)
            })

            const ctx = document.createElement('canvas').getContext('2d')
            ctx.canvas.width = 400
            ctx.canvas.height = 100
            ctx.font = 'bold 50px arial'
            ctx.fillStyle = 'red'
            ctx.fillText('Hello, world!',50,60)
            document.body.appendChild(ctx.canvas)
            
            texture = new THREE.CanvasTexture(ctx.canvas)
            const material = new THREE.MeshBasicMaterial({
                map: texture, side: THREE.DoubleSide
            })
            // material.transparent = true
            const mesh = new THREE.Mesh(new THREE.PlaneGeometry(2,.5), material)

            scene.add(mesh)
            stats = Stats()
            document.body.appendChild(stats.dom)
        }

        function animate(){
            controls.update()
            stats.update()
            texture.needsUpdate= true
            renderer.render(scene, camera)

            requestAnimationFrame(animate)
        }

        window.addEventListener('resize',onWindowResize,false)

        function onWindowResize(){
            camera.aspect = window.innerWidth/window.innerHeight
            camera.updateProjectionMatrix()
            renderer.setSize(window.innerWidth, window.innerHeight)
        }

    </script>

</body>
</html>
```
# GLTF Empty 위치에 Sprite 배치하기
![image](https://user-images.githubusercontent.com/30430227/124571780-10de0580-de83-11eb-9efd-9fd61d18d250.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
</head>
<body>

    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import {GLTFLoader} from './GLTFLoader.js'

        const ctx = document.createElement('canvas').getContext('2d')

        ctx.canvas.width = 300
        ctx.canvas.height = 300
        ctx.fillStyle = 'yellow'
        ctx.strokeStyle = 'black'
        ctx.lineWidth = 5

        roundRect(ctx,10,10, 200, 100, 20)
        ctx.fillStyle='black'
        ctx.font = 'bold 50px 바탕체'
        ctx.fillText('하이',50,80)
        
        document.body.appendChild(ctx.canvas)

// function for drawing rounded rectangles
        function roundRect(ctx, x, y, w, h, r){
            ctx.beginPath()
            ctx.moveTo(x+r,y)
            ctx.lineTo(x+w-r,y)
            ctx.quadraticCurveTo(x+w,y,x+w,y+r)
            ctx.lineTo(x+w,y+h-r)
            ctx.quadraticCurveTo(x+w, y+h,x+w-r,y+h)
            ctx.lineTo(x+r,y+h)
            ctx.quadraticCurveTo(x,y+h,x,y+h-r)
            ctx.lineTo(x,y+r)
            ctx.quadraticCurveTo(x,y,x+r,y)
            ctx.closePath()
            ctx.fill()
            // ctx.closePath()
            ctx.fill()
            ctx.stroke()
        }

        let renderer, scene, camera, controls, texture
        let empty

        init()
        animate()

        function init(){
            renderer = new THREE.WebGLRenderer({antialias:true})
            renderer.setSize(window.innerWidth, window.innerHeight)
            document.body.appendChild(renderer.domElement)

            scene = new THREE.Scene()
            scene.background = new THREE.Color('skyblue')
            const gridHelper = new THREE.GridHelper(5,10,'white','green')
            scene.add(gridHelper)

            camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, .1,100)
            camera.position.set(5,10,5)
            camera.lookAt(scene.position)
            controls = new OrbitControls(camera, renderer.domElement)

            empty = new THREE.Object3D()
            scene.add(empty)
            
            const loader = new GLTFLoader()
            loader.load('../gltfs/afo.gltf', gltf=>{
                gltf.scene.traverse(child=>{
                    if(child.name==='Empty'){
                        empty = child
                    }
                    //gltf 모델이 ambient light 안 먹힐 때
                    if(child.material)child.material.metalness =0
                })
                scene.add(gltf.scene)
                addSprite()
            },xhr=>console.log(xhr.loaded/xhr.total*100+'% 노딩')
            ,error=>console.log(error))
            {
                const ambientLight = new THREE.AmbientLight('skyblue', 1)
                scene.add(ambientLight)
                const pointLight = new THREE.PointLight()
                pointLight.position.set(-2,2,2)
                scene.add(pointLight)
            }
            console.log(empty)

        }
        function addSprite(){
            texture = new THREE.CanvasTexture(ctx.canvas)
            const material = new THREE.SpriteMaterial({
                map:texture
            })
            const sprite = new THREE.Sprite(material)
            sprite.center = new THREE.Vector2(0.1,1)
            sprite.scale.set(1.5,1.5,1.5)
            empty.add(sprite)

        }

        function animate(){
            controls.update()
            renderer.render(scene, camera)

            requestAnimationFrame(animate)
        }

        window.addEventListener('resize',onWindowResize, false)

        function onWindowResize(){
            camera.aspect = window.innerWidth/window.innerHeight
            camera.updateProjectionMatrix()
            renderer.setSize(window.innerWidth, window.innerHeight)
        }


    </script>
</body>
</html>
```
