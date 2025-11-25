<script setup>
const animationRef = ref(null)

const emit = defineEmits(['action']) // لو حاب تبعث event للـ parent لما يضغط الزر

defineExpose({ animationRef })
</script>

<template>
    <div class="absolute top-0 right-1 h-full
           w-[90vw] max-w-[400px]
           translate-x-[400px]
           bg-white
           rounded-2xl
           shadow-[0_4px_20px_rgba(0,0,0,0.07)]
           border border-[#E5E5E5]
           z-10" ref="animationRef">
        <div class="h-full p-4 sm:p-6 lg:p-8 flex flex-col gap-4">
            <!-- Title -->
            <h2 class="text-lg sm:text-xl font-semibold">
                Car's Name
            </h2>

            <!-- Text -->
            <p class="text-sm sm:text-base text-gray-700 overflow-y-auto">
                Cars have revolutionized the way we live, offering freedom, convenience, and
                the thrill of the open road. From sleek sports cars that hug curves at
                breathtaking speeds to practical sedans designed for everyday comfort,
                each vehicle embodies a blend of engineering, design, and technology.
                Modern cars are no longer just transportation; they integrate advanced
                safety systems, connectivity, and eco-friendly innovations, such as
                electric and hybrid engines, to meet the demands of a changing world.
                Whether admired for their performance, style, or efficiency, cars
                continue to capture our imagination and play an integral role in shaping
                our lives.
            </p>

            <!-- Button -->
            <div class="mt-auto">
                <button type="button" class="text-heading bg-gradient-to-r from-blue-200 via-blue-400 to-blue-500
                 hover:bg-gradient-to-br focus:ring-4 focus:outline-none
                 focus:ring-blue-300 dark:focus:ring-blue-800
                 shadow-lg shadow-blue-500/50 dark:shadow-lg dark:shadow-blue-800/80
                 font-medium rounded-xl
                 text-sm sm:text-md
                 px-6 sm:px-9 py-3 sm:py-4
                 text-center leading-5 z-10 text-white
                 hover:scale-105 transition-transform duration-300" @click="emit('action')">
                    Get Started
                </button>
            </div>
        </div>
    </div>
</template>
