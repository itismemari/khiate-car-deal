    <script setup>
    import { ArrowBigLeftDash, ArrowBigRightDash, Search } from 'lucide-vue-next';
    import { ACESFilmicToneMapping } from 'three'
    import CarCategories from '~/components/carCategories.vue';
    const cars = [
        {
            name: "Economic",
            description: "Economic cars are affordable and fuel-efficient, ideal for daily commuting and city driving.",
            price: "$20,000 - $40,000",
            features: ["Fuel-efficient", "Affordable", "Compact size"],
            models: '/models/free_1972_datsun_240k_gt/scene.gltf',
            position: { x: 0, y: 0, z: 0 }
        },
        {
            name: "sports",
            description: "Mid-size cars offer a balance of comfort, space, and performance, suitable for families and long trips.",
            price: "$30,000 - $50,000",
            features: ["Spacious interior", "Good fuel economy", "Comfortable ride"],
            models: '/models/free_porsche_911_carrera_4s/scene.gltf',
            position: { x: 0, y: 0, z: 0 }
        },
    ]

    const { $gsap } = useNuxtApp()

    const currentCarIndex = ref(0);
    const moveCamera = ref(false);
    const TextRef = ref(null);
    const CategoryRef = ref(cars[0].name);
    const header = ref(null);
    const zoomRef = ref(null);
    const zoomIn = ref(false);

    const show3D = ref(false)

    const CatRef = ref(null);
    const DetailsRef = ref(null);
    const switchBtn = ref(null);


    const eve = ref(null)
    const ry = ref(null)
    const car = ref(null)
    const tells = ref(null)
    const a = ref(null)
    const story = ref(null)
    const btn = ref(null)

    const heroSection = ref(null)

    const currentCar = computed(() => cars[currentCarIndex.value])

    const prevCar = () => {
        if (currentCarIndex.value === 0) {
            currentCarIndex.value = cars.length - 1
        } else {
            currentCarIndex.value--
        }
        CategoryRef.value = cars[currentCarIndex.value].name
    }

    const nextCar = () => {
        if (currentCarIndex.value === cars.length - 1) {
            currentCarIndex.value = 0
        } else {
            currentCarIndex.value++
        }
        CategoryRef.value = cars[currentCarIndex.value].name
    }


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
        }, "-=0.3").to(switchBtn.value, {
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
            $gsap.to(fadeTargets, {
                opacity: 0,
                y: 10,
                duration: 0.5,
                ease: "power2.inOut"
            });
        } else {
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
                <div v-if="!show3D" role="status" class="flex items-center justify-center h-full">
                    <svg aria-hidden="true"
                        class="inline w-10 h-10 w-8 h-8 text-neutral-tertiary animate-spin fill-brand"
                        viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path
                            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                            fill="currentColor" />
                        <path
                            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                            fill="currentFill" />
                    </svg>
                    <span class="sr-only">Loading...</span>
                </div>
                <ClientOnly>
                    <TresCanvas v-if="show3D" shadows window-size v-bind="rendererSettings"
                        :tone-mapping="currentToneMapping" :antialias="rendererSettings.antialias"
                        :alpha="rendererSettings.alpha" :power-preference="rendererSettings.powerPreference"
                        clearColor="#FFFFFF" @ready="onReady">
                        <modelLoader :startCamera="moveCamera" :modelURL="currentCar.models"
                            :position="currentCar.position" />
                    </TresCanvas>
                </ClientOnly>
            </div>

            <CarCategories :selectedCategory="CategoryRef" ref="CatRef" />

            <CarDetails :startCamera="moveCamera" :selectedCarText="currentCar.description"
                :selectedCarName="currentCar.name" :selectedCarPrice="currentCar.price" ref="DetailsRef" />

            <Header ref="header" />

            <button type="button" class="absolute bottom-4 left-0 lg:left-4  opacity-0 translate-y-10
         w-8 h-8 
         lg:w-16 lg:h-16
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

            <div ref="switchBtn" class="absolute top-18 lg:top-4 left-4 flex gap-2 z-50 opacity-0 -translate-y-10">
                <button type="button" @click="prevCar"
                    class="text-gray-500 bg-transparent border border-gray-500 px-4 py-2 rounded-lg cursor-pointer hover:text-white bg-black/40 hover:bg-black/60 backdrop-blur transition">
                    <component :is="ArrowBigLeftDash" :size="20" />
                </button>
                <button type="button" @click="nextCar"
                    class="text-gray-500 bg-transparent border border-gray-500 px-4 py-2 rounded-lg cursor-pointer hover:text-white bg-black/40 hover:bg-black/60 backdrop-blur transition">
                    <component :is="ArrowBigRightDash" :size="20" />
                </button>
            </div>

        </div>



    </template>

<style scoped>
.reforma {
    font-family: "Notable", sans-serif;
    font-weight: bold;
    font-style: normal;
}
</style>