<script setup>
import { useLoop, useLoader } from '@tresjs/core'
import { ref, shallowRef, watch } from 'vue'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { AnimationMixer, Vector3, Box3 } from 'three'
const { onBeforeRender } = useLoop()

const { $gsap } = useNuxtApp()

const camera = ref(null)
const ObjectRef = shallowRef(null)
const mixer = ref(null)

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
        default: () => ({ x: 0, y: 0, z: 0 }),
    }
})

const { state: model, isLoading, error, progress } = useLoader(
    GLTFLoader,
    () => props.modelURL
)

function centerModel(model) {
    const box = new Box3().setFromObject(model);
    const center = new Vector3();
    box.getCenter(center);
    model.position.sub(center);
}

watch(isLoading, (loading) => {
    if (loading) console.log('Loading model...')
})

watch(progress, (prog) => {
    console.log(`Loading: ${prog.percentage}%`)
})


watch(model, (gltf) => {
    if (!gltf) return

    ObjectRef.value = gltf.scene

    centerModel(gltf.scene)

    mixer.value = new AnimationMixer(gltf.scene)

    gltf.animations.forEach((clip) => {
        mixer.value?.clipAction(clip).play()
    })
})

watch(
    () => props.startCamera,
    (start) => {
        if (!start || !camera.value || !model.value) return

        const pos = new Vector3()
        model.value.scene.getWorldPosition(pos)

        const tl = $gsap.timeline()

        tl.to(camera.value.position, {
            x: pos.x + 2,
            y: pos.y + 2,
            z: pos.z + 2,
            duration: 1.5,
            ease: 'sine.out',
            onUpdate: () => camera.value.lookAt(pos),
        }).to(camera.value.position, {
            x: pos.x - 1,
            y: pos.y + 1,
            z: pos.z + 3,
            duration: 1.5,
            ease: 'sine.out',
            onUpdate: () => camera.value.lookAt(pos),
        })
    }
)

onBeforeRender(({ delta, elapsed }) => {
    if (ObjectRef.value) {
        ObjectRef.value.rotation.y = elapsed * 0.2
    }
    mixer.value?.update(delta)
})
</script>

<template>
    <TresPerspectiveCamera ref="camera" :position="[10, 10, 10]" :look-at="[0, 0, 0]" />

    <TresAmbientLight :intensity="0.5" />

    <TresHemisphereLight color="#ffffff" groundColor="#222222" :intensity="0.4" :position="[0, 10, 0]" />

    <TresDirectionalLight castShadow :position="[5, 10, 5]" :intensity="1.2" :shadow-mapSize="[2048, 2048]"
        :shadow-camera-near="0.5" :shadow-camera-far="50" :shadow-camera-left="-10" :shadow-camera-right="10"
        :shadow-camera-top="10" :shadow-camera-bottom="-10" />

    <TresDirectionalLight :position="[-5, 6, -3]" :intensity="1.7" />

    <TresPointLight :position="[0, 5, -6]" :intensity="0.9" :distance="20" />

    <TresSpotLight :position="[0, 12, 5]" :intensity="1" :angle="0.6" :penumbra="0.4" :castShadow="true" />

    <OrbitControls :enableZoom="false" :enablePan="true" />

    <primitive v-if="!isLoading && model?.scene" ref="ObjectRef" :object="model.scene"
        :position="[position.x, position.y, position.z]" castShadow receiveShadow />
</template>
