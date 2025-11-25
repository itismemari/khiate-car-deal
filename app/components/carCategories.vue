    <template>
        <div class="absolute w-full top-4 left-0 flex justify-center opacity-0 translate-y-10 z-50" ref="animationRef">
            <div
                class="flex flex-wrap items-center justify-center gap-4 md:gap-8 h-12 bg-white shadow-xl px-10 rounded-xl w-[300px] border border-gray-100">
                <ul class="flex items-center gap-6">
                    <li v-for="item in menuItems" :key="item.id"
                        class="relative py-2 px-3 rounded-lg border border-gray-200 hover:scale-110 transition-all duration-200 cursor-pointer"
                        :class="{
                            'bg-blue-500 text-white': selectedCategory.toLowerCase() === item.label.toLowerCase(),
                            'bg-gray-100': selectedCategory.toLowerCase() !== item.label.toLowerCase()
                        }" @mouseenter="showLabel(item.id)" @mouseleave="hideLabel"
                        @click="$emit('update:selectedCategory', item.label)">

                        <div class="relative flex flex-col items-center">
                            <!-- Icon -->
                            <component :is="item.icon" :size="20" />

                            <!-- Label on hover -->
                            <span v-if="hoveredId === item.id"
                                class="absolute -bottom-8 left-1/2 transform -translate-x-1/2 px-2 py-1 text-sm rounded bg-gray-800 text-white shadow z-50 whitespace-nowrap transition-opacity duration-200">
                                {{ item.label }}
                            </span>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </template>

<script setup>
import { Car } from 'lucide-vue-next'
import { ref } from 'vue'

const props = defineProps({
    selectedCategory: {
        type: String,
        default: 'luxury'
    }
})

const animationRef = ref(null)
const hoveredId = ref(null)

defineExpose({ animationRef })


const menuItems = [
    { id: 1, label: 'luxury', icon: Car },
    { id: 2, label: 'Sports', icon: Car },
    { id: 3, label: 'Economic', icon: Car },
    { id: 4, label: 'Travel', icon: Car },
]

const showLabel = (id) => hoveredId.value = id
const hideLabel = () => hoveredId.value = null
</script>
