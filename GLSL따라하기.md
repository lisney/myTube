# 삼각형
![image](https://user-images.githubusercontent.com/30430227/122667063-de2ddf00-d1eb-11eb-9020-b58620584c6b.png)

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
<container id="c"></container>

</script>

<script type="module">
    import * as THREE from './three.module.js'

    let renderer, scene, camera
    const canvas = document.querySelector('#c')

    const vShader =`
        void main(){
			gl_Position = projectionMatrix * modelViewMatrix * vec4(position,1.0);
        }
    `
    const fShader =`
        void main(){
            gl_FragColor = vec4(1.0,0.0,0,1.0);
        }
    `

    init()

    function init(){
        renderer = new THREE.WebGLRenderer()
        renderer.setSize(600,300)
        canvas.appendChild(renderer.domElement)

        scene = new THREE.Scene()

        camera = new THREE.PerspectiveCamera(75, 2, .1, 10)
        camera.position.set(0,0,4)

        const planeGeometry = new THREE.PlaneBufferGeometry(2,2,1,1)
        //레퍼 지오메트리 생성, 현재로선 빈 껍데기
        const geometry = new THREE.BufferGeometry()
        //세 개의 꼭지점 좌표
        const positionData =[
            0,0,0,
            2,0,0,
            0,2,0
        ]
        //세 꼭지점으로 배열화
        const positionArray = new Float32Array(positionData)

        const positionAttribute = new THREE.BufferAttribute(
            positionArray, //래핑하는 실제 데이터
            3, //itemSize : 요소의 크기
            false //정규화: 색상의 경우 true 여야 함
        )
        // 데이터를 레퍼로 전달
        geometry.setAttribute('position', positionAttribute)

        // const material = new THREE.MeshBasicMaterial()
        const material = new THREE.ShaderMaterial({
            vertexShader: vShader,
            fragmentShader: fShader
        })
        const plane = new THREE.Mesh(geometry, material)
        scene.add(plane)

        renderer.render(scene, camera)

    }
</script>
</body>
</html>
```

# 글로우 쉐이더
![image](https://user-images.githubusercontent.com/30430227/124870830-40fde380-dffe-11eb-9fab-149f76b3fe5f.png)
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
<canvas id="c"></canvas>
<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'

    let renderer, scene, camera, controls
    const canvas = document.querySelector('#c')

    const vShader =`
    uniform vec3 viewVector;
uniform float c;
uniform float p;
varying float intensity;
void main() 
{
    vec3 vNormal = normalize( normalMatrix * normal );
	vec3 vNormel = normalize( normalMatrix * viewVector );
	intensity = pow( c - dot(vNormal, vNormel), p );
	
    gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );
}
    `
    const fShader =`
    uniform vec3 glowColor;
varying float intensity;
void main() 
{
	vec3 glow = glowColor * intensity;
    gl_FragColor = vec4( glow, 1.0 );
}
    `

// const vShader =`
//         void main(){
// 			gl_Position = projectionMatrix * modelViewMatrix * vec4(position,1.0);
//         }
//     `
//     const fShader =`
//         void main(){
//             gl_FragColor = vec4(1.0,0.0,0,1.0);
//         }
//     `
    init()
    render()

    function init(){
        renderer = new THREE.WebGLRenderer({canvas})
        
        scene = new THREE.Scene()

        camera = new THREE.PerspectiveCamera(50, 2, .1, 10)
        camera.position.set(2,2,4)
        controls = new OrbitControls(camera, canvas)
        controls.target = scene.position

        {
            const light = new THREE.PointLight()
            light.position.set(0,5,0)
            scene.add(light)
        }

        let geometry = new THREE.BufferGeometry()

        const positionData=[
            0,0,0,
            2,0,0,
            0,2,0
        ]
        const positionArray = new Float32Array(positionData)

        const positionAttribute = new THREE.BufferAttribute(
            positionArray,
            3,
            false
        )

        geometry.setAttribute('position', positionAttribute)

        geometry = new THREE.BoxGeometry()

        // const material = new THREE.MeshBasicMaterial()

        const material = new THREE.ShaderMaterial({
            uniforms: 
		{ 
			"c":   { type: "f", value: 1.0 },
			"p":   { type: "f", value: 1.4 },
			glowColor: { type: "c", value: new THREE.Color(0xffff00) },
			viewVector: { type: "v3", value: camera.position }
		},
            vertexShader: vShader,
            fragmentShader:fShader,
            blending: THREE.AdditiveBlending,
            transparent: true
        })

        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        const spGeometry = new THREE.SphereGeometry(1,32,16)

        const sphere = new THREE.Mesh(spGeometry, material.clone())
        sphere.scale.multiplyScalar(1.2)
        scene.add(sphere)

    }

    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!=canvas.width||height!=canvas.height

        if(needResize){
            renderer.setSize(width, height, false)
        }

        return needResize
    }

    function render(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.clientWidth/canvas.clientHeight
            camera.updateProjectionMatrix()
        }
        controls.update()
        renderer.render(scene, camera)

        requestAnimationFrame(render)
    }
</script>
</body>
</html>
```
