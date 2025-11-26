<script setup>
const animationRef = ref(null)

const emit = defineEmits(['action'])

defineExpose({ animationRef })
</script>

<template>
    <div ref="animationRef" class="
      fixed
      inset-x-4 bottom-18
      sm:inset-x-6 sm:bottom-6
      lg:inset-y-0 lg:top-2 lg:right-8 lg:left-auto
      w-auto lg:w-[380px] xl:w-[400px]
      translate-x-[400px]
      bg-white
      rounded-2xl
      shadow-[0_4px_20px_rgba(0,0,0,0.07)]
      border border-[#E5E5E5]
      z-10
      max-h-[70vh] sm:max-h-[75vh] lg:max-h-[85vh]
      flex flex-col
    ">
        <div class="h-full p-4 sm:p-6 lg:p-8 flex flex-col gap-4">
            <!-- Title -->
            <h2 class="text-base sm:text-lg lg:text-xl font-semibold">
                Car's Name
            </h2>

            <!-- Text -->
            <p class="
          text-xs sm:text-sm lg:text-base
          text-gray-700
          flex-1
          overflow-y-auto
          pr-1 sm:pr-2
        ">
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
            <div class="mt-2 pt-2 border-t border-gray-100">
                <button type="button" class="
            text-heading bg-gradient-to-r from-blue-200 via-blue-400 to-blue-500
            hover:bg-gradient-to-br
            focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800
            shadow-lg shadow-blue-500/50 dark:shadow-lg dark:shadow-blue-800/80
            font-medium rounded-xl
            text-sm sm:text-base
            px-5 sm:px-7 py-2.5 sm:py-3
            text-center leading-5 z-10 text-white
            hover:scale-105 transition-transform duration-300
            w-full sm:w-auto
          " @click="emit('action')">
                    Get Started
                </button>
            </div>
        </div>
    </div>
</template>
