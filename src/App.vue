<template>

  <input v-model="ranger" step=".2" style="position: absolute; width: 90vw; margin: 0 5vw; top: 80vh" min="1" max="20" type="range"/>

  <Renderer  ref="rendererC" antialias :orbit-ctrl="{ enableDamping: true }" resize="window">
    <Camera :position="{ x: 0, y: 0, z: 10 }" />
    <Scene >
      <PointLight :distance="100" :intensity="1" :position="{ x: -3, y:0, z: 8 }" />
      <AmbientLight :intensity=".2"/>

      <Group ref="cyberC">

        <Torus :position="{y: 2}" :radius=".5"><LambertMaterial :color="0x2bce93"/></Torus>

        <Box><LambertMaterial :color="0xfbae43"/></Box>

      </Group>

      <Group :position="{x: -3}" ref="cyberD">

        <Icosahedron :position="{y: 2}" :radius=".5"><LambertMaterial :color="0x666293"/></Icosahedron>

        <Cylinder :radiusBottom="1.5"><LambertMaterial :color="0xf57373"/></Cylinder>

      </Group>

      <Group :position="{x: 3}" ref="cyberE">

        <Dodecahedron :position="{y: 2}"><LambertMaterial :color="0x666293"/></Dodecahedron>

        <Sphere :radius=".7"><LambertMaterial :color="0xf57373"/></Sphere>

      </Group>


      <EffectComposer>
        <RenderPass/>
        <UnrealBloomPass :strength=".3" :threshold=".1" :radius="1.1"/>
        <UnrealBloomPass :strength=".3" :threshold=".1" :radius=".1"/>
        <HalftonePass ref="passC" :shape="4" :radius="ranger"/>
        <SMAAPass/>


      </EffectComposer>

    </Scene>
  </Renderer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { SMAAPass, UnrealBloomPass, HalftonePass, EffectComposer, RenderPass, AmbientLight, GltfModel, Box, Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs'
const rendererC = ref()
const cyberC = ref()
const passC = ref()
const cyberD = ref()
const cyberE = ref()
const ranger = ref(1)

onMounted(() => {
  const renderer = rendererC.value
  const cyber = cyberC.value.group
  const cyber2 = cyberD.value.group
  const cyber3 = cyberE.value.group

  renderer.onBeforeRender(() => {
    cyber.rotation.y += .01
    cyber2.rotation.y += .02
    cyber3.rotation.y += .015
  })
})
</script>

<style>
body {
  margin: 0;
}
canvas {
  display: block;
}
</style>