<template>
  <div>
  <canvas ref="canvas"></canvas>
  <div id="app">
      <div
        id="container" 
        class="absolute text-white text-center w-full max-w-2xl px-6"
        style="top: 50%;transform: translate(50%, -50%); left: 15%"
      >
        <h1 
          id="HiYall" 
          class="font-maven text-2.5xl uppercase opacity-0"
          style="transform: translateY(30px)"
          >
          Howdy yall,
          I'm Brady, and this is my site. click around, it should do stuff.
        </h1>
          <p 
            id="description"
            class="font-maven text-5xl opacity: 0"
            style="transform: translateY(30px)"
            >
            Aspiring to Inspire change through creativity: Designing for a better future

          </p>
        

          <button>
          <a
          id="button1"
          href="https://www.bchoatedesign.com/home/case-studies"
            class="border px-8 py-2 rounded-lg text-4xl font-maven mt-12 hover:bg-white hover:text.gray-800 inline-block"
            style="transform: translateY(30px)"
            >
            Work Projects  
          </a>
        </button>
          <button>
          <a
          id="button2"
          href="https://www.bchoatedesign.com/"
            class="border px-8 py-2 rounded-lg text-4xl font-maven mt-12 hover:bg-white hover:text.gray-800 inline-block"
            style="transform: translateY(30px)"
            >
            External
            Portfolio
            Site
          </a>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import gsap from 'gsap'
import {
  PlaneGeometry,
  BufferAttribute,
  Raycaster,
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  MeshPhongMaterial,
  DoubleSide,
  FlatShading,
  Mesh,
  DirectionalLight,
  BufferGeometry,
  PointsMaterial,
  Float32BufferAttribute,
  Points
} from 'three'
//import OrbitControls from 'orbit-controls-es6'
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"

export default {
  mounted(){
   const dat = require('dat.gui')

// howdy yall
//@ts-check

const gui = new dat.GUI()
const world = {
plane: {
  width: 256,
  height: 256,
  widthSegments: 128,
  heightSegments: 128
  }
}
gui.add(world.plane, 'width', 1, 64, ).onChange(generatePlane)
gui.add(world.plane, 'height', 1, 64, ).onChange(generatePlane)

gui.add(world.plane, 'widthSegments', 1, 64, ).onChange(generatePlane)  
gui.add(world.plane, 'heightSegments', 1, 64, ).onChange(generatePlane)  

function generatePlane(){
planeMesh.geometry.dispose()
planeMesh.geometry = new PlaneGeometry(
  world.plane.height, 
  world.plane.height, 
  world.plane.widthSegments, 
  world.plane.heightSegments
  )

//vertice position randomization
const {array} = planeMesh.geometry.attributes.position
const randomValues = []
for (let i = 0; i < array.length; i ++) {
if (i % 3 === 0) {
const x = array[i]
const y = array[i + 1]
const z = array[i + 2]

array[i] = x + (Math.random() -0.5) * 3
array[i + 1] = y + (Math.random() -0.5) * 3
array[i + 2] = z + (Math.random() -0.5) * 3
}
randomValues.push(Math.random() * Math.PI * 2)
} 

planeMesh.geometry.attributes.position.randomValues = randomValues
planeMesh.geometry.attributes.position.originalPosition =
planeMesh.geometry.attributes.position.array
//colorsnext
const colors = []
for (let i = 0; i < planeMesh.geometry.attributes.position.count; i++){
colors.push(0, 0.19, 0.4)
}

planeMesh.geometry.setAttribute(
'color',
new BufferAttribute(new Float32Array(colors),3)
  )
}

const raycaster = new Raycaster()
const scene = new Scene()
const camera = new PerspectiveCamera(
75, 
innerWidth / innerHeight,
0.1,
1000
)
const renderer = new WebGLRenderer({
  canvas: this.$refs.canvas
})

renderer.setSize(innerWidth, innerHeight)
renderer.setPixelRatio(devicePixelRatio)
//document.body.appendChild(renderer.domElement)

var controls = new OrbitControls(camera, renderer.domElement)
camera.position.z = 50


const planeGeometry = new PlaneGeometry(
world.plane.width,
world.plane.height,
world.plane.widthSegments,
world.plane.heightSegments
)
const planeMaterial = new MeshPhongMaterial({
  //color: 0xff0000,
  side: DoubleSide,
  flatShading: true,
  vertexColors: true
})
const planeMesh = new Mesh(planeGeometry, planeMaterial)
scene.add(planeMesh)
generatePlane()

//lighting
const light = new DirectionalLight(0xffffff, 1)
light.position.set(0, 1, 1)
scene.add(light)

const light2 = new DirectionalLight(0xffffff, 1)
light.position.set(0, 2, 10)
scene.add(light)

const backLight = new DirectionalLight(0xffffff, 1)
light.position.set(0, -1, -1)
scene.add(backLight)

const starGeometry = new BufferGeometry()
const starMaterial = new PointsMaterial({
color: 0xffffff
})
console.log(starGeometry);
console.log(starMaterial);


const starVertices = []
for (let i = 0; i < 50000; i++) {
const x = (Math.random() - .05) * 5000
const y = (Math.random() - .05) * 5000
const z = (Math.random() - .05) * 5000
starVertices.push(x, y, z)
}
console.log(starVertices);
starGeometry.setAttribute('position',
   new Float32BufferAttribute(starVertices, 3)
   )

const stars = new Points(starGeometry, starMaterial)
scene.add(stars)  

const mouse = {
x: undefined,
y: undefined
}
//console.log(starVertices);
//console.log(starGeometry);
//console.log(starMaterial);
let frame = 0

function animate(){
requestAnimationFrame(animate)
renderer.render(scene, camera)
//animation loop
raycaster.setFromCamera(mouse, camera)
frame += 0.01

const { 
array,
originalPosition,
randomValues 
} = planeMesh.geometry.attributes.position
for (let i = 0; i < array.length; i++) {
const element = array[i];
// x
array[i] = originalPosition[i] + 
  Math.cos(frame + randomValues[i]) * 0.02
// y
array[i + 1] = originalPosition[i +1] + 
  Math.sin(frame + randomValues[i + 1]) * 0.02
}

planeMesh.geometry.attributes.position.needsUpdate = true

const intersects = raycaster.intersectObject(planeMesh)
if (intersects.length > 0) {
  const {color} = intersects[0].object.geometry.attributes

  //vertice 1
  color.setX(intersects[0].face.a, 0.1)
  color.setY(intersects[0].face.a, 0.3)
  color.setZ(intersects[0].face.a, 0.3)
  //vertice 2
  color.setX(intersects[0].face.b, 0.1)
  color.setY(intersects[0].face.b, 0.5)
  color.setZ(intersects[0].face.b, 1)
    //vertice 3
  color.setX(intersects[0].face.c, 0.1)
  color.setY(intersects[0].face.c, 0.5)
  color.setZ(intersects[0].face.c, 1)
//needsUpdate = true
intersects[0].object.geometry.attributes.color.needsUpdate = true

  const initialColor = {
    r: 0,
    g: 0.19,
    b: 0.4
  }

  const hoverColor = {
    r: 0.1,
    g: 0.5,
    b: 1
  }

  gsap.to(hoverColor, {
    r: initialColor.r,
    g: initialColor.g,
    b: initialColor.b,
    onUpdate: () => {
      //vertice 1
    color.setX(intersects[0].face.a, hoverColor.r)
    color.setY(intersects[0].face.a, hoverColor.g)
    color.setZ(intersects[0].face.a, hoverColor.b)
    //vertice 2
    color.setX(intersects[0].face.b, hoverColor.r)
    color.setY(intersects[0].face.b, hoverColor.g)
    color.setZ(intersects[0].face.b, hoverColor.b)
      //vertice 3
    color.setX(intersects[0].face.c, hoverColor.r)
    color.setY(intersects[0].face.c, hoverColor.g)
    color.setZ(intersects[0].face.c, hoverColor.b)
    color.needsUpdate = true
    }
  })
}

stars.rotation.x += 0.0003
}

animate()

//listeners
addEventListener('mousemove', (event) => 
{
mouse.x = (event.clientX / innerWidth) * 2 - 1
mouse.y = -(event.clientY / innerHeight) * 2 + 1
//console.log(event.clientY)
//console.log(mouse)
})

gsap.to('#BradyChoate', {
opacity: 1,
duration: 2,
y: 0,
ease: 'expo'
})
gsap.to('#description', {
opacity: 1,
duration: 2,
delay: 0.3,
y: 0,
ease: 'expo'
})
gsap.to('#button1', {
opacity: 1,
duration: 2,
delay: 0.6,
y: 0,
ease: 'expo'
})
//button1
document.querySelector('#button1')?.addEventListener('click', (e) => {
e.preventDefault()
gsap.to('#container', {
  opacity: 0,
})

gsap.to(camera.position, {
z: 25,
ease: 'power3.inOut',
duration: 2
})
gsap.to(camera.rotation, {
x: 1.57,
ease: 'power3.inOut',
duration: 2
})

gsap.to(camera.position, {
y: 1025,
ease: 'power3.in',
duration: 1.5,
delay: 2,
onComplete: () => {
  this.$router.push('/work')
}
})
})
//button2
document.querySelector('#button2')?.addEventListener('click', (e) => {
e.preventDefault()
gsap.to('#container', {
  opacity: 0,
})

gsap.to(camera.position, {
z: 25,
ease: 'power3.inOut',
duration: 2
})
gsap.to(camera.rotation, {
x: 1.57,
ease: 'power3.inOut',
duration: 2
})

gsap.to(camera.position, {
y: 1025,
ease: 'power3.in',
duration: 1.5,
delay: 2,
onComplete: () => {
  window.location = 'https://www.bchoatedesign.com/'
}
})
})

addEventListener('resize', () => {
camera.aspect = innerWidth / innerHeight
camera.updateProjectionMatrix()
renderer.setSize(innerWidth, innerHeight)
})
  }
}
</script>
