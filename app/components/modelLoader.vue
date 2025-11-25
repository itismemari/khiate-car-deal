<script setup>
import { useLoop } from '@tresjs/core'
import { ref } from 'vue'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { AnimationMixer } from 'three'
import * as THREE from 'three'

const { $gsap } = useNuxtApp()

const camera = ref(null)

const props = defineProps({
    startCamera: {
        type: Boolean,
        required: true,
    },
    modelURL: {
        type: String,
        required: true,
    },
    position: {
        type: Object,
        default: () => ({ x: 0, y: 0, z: 0 })
    }
})

// Reference to our donut mesh for animation
const ObjectRef = shallowRef(null)

const mixer = ref(null)

const { state: model, isLoading, error, progress } = useLoader(
    GLTFLoader,
    props.modelURL
)


watch(isLoading, (loading) => {
    if (loading) { console.log('Loading model...') }
})

watch(progress, (prog) => {
    console.log(`Loading: ${prog.percentage}%`)
})

watch(model, (gltf) => {
    if (!gltf) return

    mixer.value = new AnimationMixer(gltf.scene)


    gltf.animations.forEach((clip) => {
        mixer.value?.clipAction(clip).play()
    })
})

watch(() => props.startCamera, (start) => {
    if (start && camera.value) {
        const pos = new THREE.Vector3();
        model.value.scene.getWorldPosition(pos)

        const tl = $gsap.timeline()

        tl.to(camera.value.position, {
            x: pos.x + 2,
            y: pos.y + 2,
            z: pos.z + 2,
            duration: 1.5,
            ease: "sine.out",
            onUpdate: () => camera.value.lookAt(pos)
        })
            .to(camera.value.position, {
                x: pos.x - 1,
                y: pos.y + 1,
                z: pos.z + 3,
                duration: 1.5,
                onUpdate: () => camera.value.lookAt(pos)
            })
    }
})


// Animation loop
const { onBeforeRender } = useLoop()

onBeforeRender(({ delta, elapsed }) => {
    if (ObjectRef.value) {
        ObjectRef.value.rotation.y = elapsed * 0.2
        mixer.value?.update(delta)
    }
})

</script>

<template>
    <TresPerspectiveCamera ref="camera" :position="[10, 10, 10]" :look-at="[0, 0, 0]" />

    <TresAmbientLight :intensity="0.7" />

    <TresDirectionalLight castShadow :position="[5, 10, 5]" :intensity="1.2" :shadow-mapSize="[2048, 2048]"
        :shadow-camera-near="0.5" :shadow-camera-far="50" :shadow-camera-left="-10" :shadow-camera-right="10"
        :shadow-camera-top="10" :shadow-camera-bottom="-10" />
    <OrbitControls :enableZoom="false" :enablePan="true" />

    <!-- Model -->
    <primitive v-if="!isLoading && model?.scene" ref="ObjectRef" :object="model.scene"
        :position="[props.position.x, props.position.y, props.position.z]" castShadow receiveShadow />
</template>
