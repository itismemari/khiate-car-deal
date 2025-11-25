<template>
    <div class="absolute bottom-4 left-0 w-full flex justify-center opacity-0 translate-y-10 z-10" ref="animationRef">
        <div class="flex h-12 sm:h-14 md:h-16 items-center justify-center
             bg-white/90 backdrop-blur
             shadow-xl px-4 sm:px-6 md:px-10
             rounded-2xl
             w-[260px] sm:w-[300px] md:w-[340px]
             border border-gray-100">
            <ul class="flex items-center gap-4 sm:gap-6 md:gap-8">
                <li v-for="item in menuItems" :key="item.id" class="relative bg-gray-100 p-2.5 sm:p-3 rounded-xl
                 border border-gray-200
                 hover:scale-110 transition-transform duration-200
                 cursor-pointer" @mouseenter="showLabel(item.id)" @mouseleave="hideLabel"
                    @click="handleClick(item.id)">
                    <div class="relative flex flex-col items-center">
                        <!-- Icon -->
                        <component :is="item.icon" class="w-4 h-4 sm:w-5 sm:h-5" />

                        <!-- Label -->
                        <span v-if="hoveredId === item.id" class="hidden sm:block absolute -top-8 left-1/2 transform -translate-x-1/2
                     px-2 py-1 text-[11px] sm:text-xs rounded
                     bg-gray-800 text-white shadow z-50 whitespace-nowrap">
                            {{ item.label }}
                        </span>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { Box, Home, Mail, Settings } from 'lucide-vue-next'

const hoveredId = ref(null)
const animationRef = ref(null)

defineExpose({
    animationRef
})

const menuItems = [
    { id: 1, label: 'Home', icon: Home },
    { id: 2, label: 'Projects', icon: Box },
    { id: 3, label: 'Contact', icon: Mail },
    { id: 4, label: 'Settings', icon: Settings },
]

const showLabel = (id) => {
    hoveredId.value = id
}

const hideLabel = () => {
    hoveredId.value = null
}

const handleClick = (id) => {
    console.log(`Item ${id} clicked!`)
    // navigation logic here
}
</script>
