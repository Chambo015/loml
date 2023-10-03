<template>
    <h1 v-if="colon" class="timer-char colon">:</h1>
    <div v-else ref="refEl" class="timer-char number">
        <div  class="timer-char-slider" :style="{top: top}">
            <span v-for="(i, idx) of 10" :key="idx" :class="['timer-char-slider-option', {active: number === idx}]">{{idx}}</span>
        </div>
    </div>
</template>

<script setup lang="ts">
import {computed, ref} from 'vue'

const props = defineProps({
    char: String
})

const refEl = ref<HTMLDivElement>()

const colon = computed(() => {
    return props.char === ":"
})

const number = computed(() => {
    return parseInt(props.char!)
})

const height = computed(() => refEl.value ? refEl.value.offsetHeight : 0) 
const top = computed(() => `${number.value * height.value * -1}px`)

</script>

<style scoped>

</style>