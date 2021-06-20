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
