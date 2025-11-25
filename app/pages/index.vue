    <script setup>
    import { Search } from 'lucide-vue-next';
    import { ACESFilmicToneMapping } from 'three'
    import CarCategories from '~/components/carCategories.vue';

    const { $gsap, $ScrollTrigger } = useNuxtApp()

    const moveCamera = ref(false);
    const TextRef = ref(null);
    const CategoryRef = ref("Economic");
    const header = ref(null);
    const zoomRef = ref(null);
    const zoomIn = ref(false);

    const show3D = ref(false)

    const CatRef = ref(null);
    const DetailsRef = ref(null);


    const eve = ref(null)
    const ry = ref(null)
    const car = ref(null)
    const tells = ref(null)
    const a = ref(null)
    const story = ref(null)
    const btn = ref(null)

    const heroSection = ref(null)


    const isHighPerformanceDevice = () => {
        if (typeof navigator !== 'undefined' && navigator.hardwareConcurrency) {
            return navigator.hardwareConcurrency > 4
        }
        return false // safe fallback on server
    }

    const rendererSettings = {
        alpha: true,
        antialias: isHighPerformanceDevice(),
        powerPreference: isHighPerformanceDevice() ? 'high-performance' : 'default'
    }

    const currentToneMapping = ref(ACESFilmicToneMapping)


    const onReady = (context) => {
        console.log('Renderer is ready:', context.renderer.instance)
        // Your ready logic here
    }

    onMounted(() => {

        setTimeout(() => {
            show3D.value = true
        }, 1000)
        const letters = [eve.value, ry.value, car.value, tells.value, a.value, story.value]

        const tl = $gsap.timeline()

        tl.fromTo(
            letters,
            { opacity: 0, y: '1rem' },
            { opacity: 1, y: '0rem', duration: 0.5, ease: 'power1.out', stagger: 0.3 }
        )
            .fromTo(
                btn.value,
                { scale: 0.8, opacity: 0 },
                { scale: 1, opacity: 1, ease: 'elastic.out' },
                ">0.2" // start 0.2s after previous animation ends
            )
    })

    watch(moveCamera, (newVal) => {
        if (!newVal || !TextRef.value || !CatRef.value || !DetailsRef.value) return;

        const tl = $gsap.timeline()

        tl.to(TextRef.value, {
            opacity: 0,
            duration: 0.8,
            ease: "power2.inOut",
            onComplete: () => {
                TextRef.value.style.display = "none";
            }
        }).to(
            CatRef.value.animationRef,
            {
                opacity: 1,
                y: 0,
                duration: 0.8,
                ease: "power2.out"
            }, "-=0.5"
        ).to(DetailsRef.value.animationRef, {
            x: 0,
            duration: 0.8,
            ease: "power2.out"
        }, "-=0.7").to(
            header.value.animationRef, {
            opacity: 1,
            y: 0,
            duration: 0.8,
            ease: "power2.out"
        }, "-=0.4").to(zoomRef.value, {
            opacity: 1,
            y: 0,
            duration: 1,
            ease: "power2.inOut"
        }, "-=0.3")
    });


    watch(zoomIn, (zoom) => {
        const fadeTargets = [
            DetailsRef.value?.animationRef,
            header.value?.animationRef,
            CatRef.value?.animationRef
        ].filter(Boolean);

        if (zoom) {
            // zoomIn = true → اخفِ العناصر
            $gsap.to(fadeTargets, {
                opacity: 0,
                y: 10,
                duration: 0.5,
                ease: "power2.inOut"
            });
        } else {
            // zoomIn = false → رجّعهم
            $gsap.to(fadeTargets, {
                opacity: 1,
                y: 0,
                duration: 0.5,
                ease: "power2.inOut"
            });
        }
    })


</script>

    <template>
        <div class="relative w-screen h-screen bg-[#F5F5F7] overflow-hidden m-0" ref="heroSection">

            <div class="absolute inset-0 flex flex-col justify-center px-4 reforma items-center text-2xl lg:text-6xl text-center z-10 gap-8 pointer-events-none;
" ref="TextRef">
                <!-- First line -->
                <div class="flex flex-wrap justify-center text-[#1D1D1F]">
                    <span ref="eve">Eve</span>
                    <span ref="ry">ry</span>
                    <span ref="car" class="pl-4">Car</span>
                    <span ref="tells" class="pl-4">tells</span>
                    <span ref="a" class="pl-4">A</span>
                    <span ref="story" class="pl-4">story</span>
                </div>

                <button type="button"
                    class="text-heading bg-gradient-to-r from-blue-200 via-blue-400 to-blue-500 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 shadow-lg shadow-blue-500/50 dark:shadow-lg dark:shadow-blue-800/80 font-medium rounded-xl text-xl px-12 py-5 text-center leading-5 z-1 text-white hover:scale-105 transition-transform duration-300"
                    ref="btn" @click="moveCamera = true" style="pointer-events:auto">Get Started</button>
            </div>

            <div class="w-full h-full bg-transparent z-0">
                <TresCanvas shadows window-size v-bind="rendererSettings" :tone-mapping="currentToneMapping"
                    :antialias="rendererSettings.antialias" :alpha="rendererSettings.alpha"
                    :power-preference="rendererSettings.powerPreference" clearColor="#FFFFFF" @ready="onReady">
                    <modelLoader v-if="show3D" :startCamera="moveCamera"
                        :modelURL="'/models/free_porsche_911_carrera_4s/scene.gltf'" :position="{ x: 0, y: 0, z: 0 }" />
                </TresCanvas>
            </div>

            <CarCategories :selectedCategory="CategoryRef" ref="CatRef" />

            <carDetails :startCamera="moveCamera" ref="DetailsRef" />

            <Header ref="header" />

            <button type="button" class="absolute bottom-4 left-4  opacity-0 translate-y-10
         w-16 h-16 
         rounded-full 
         border border-blue-500 
         text-blue-500 
         bg-transparent 
         hover:bg-blue-500 hover:text-white 
         transition-all duration-300 
         flex items-center justify-center 
         text-xl font-medium 
         shadow-lg z-10" ref="zoomRef" @click="zoomIn = !zoomIn">
                <component :is="Search" :size="20" />
            </button>

        </div>



    </template>

<style scoped>
.reforma {
    font-family: "Notable", sans-serif;
    font-weight: bold;
    font-style: normal;
}
</style>