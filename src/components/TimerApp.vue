<template>
    <div id="timer">
        <div id="timer-text">
            <TimerChar v-for="(char, index) of getTime" :key="index" :char="char" />
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'
import TimerChar from './TimerChar.vue'

const props = defineProps({
    date: {
        type: String,
        default: '2023-07-01T02:40:00'
    }
})

const formatSegment = (segment: number): string => {
    return segment < 10 ? `0${segment}` : `${segment}`
}

const nowDate = ref<Date>(new Date())
const timeDiff = computed(() => Math.abs(nowDate.value.getTime() - new Date(props.date).getTime()))
const getTime = computed(() => {
    const days = Math.ceil(timeDiff.value / (1000 * 3600 * 24)),
        hours = Math.ceil(timeDiff.value / (1000 * 3600)) % 24,
        minutes = Math.ceil(timeDiff.value / (1000 * 60)) % 60,
        seconds = Math.ceil(timeDiff.value / 1000) % 60
    console.log(`${formatSegment(days)}:${formatSegment(hours)}:${formatSegment(minutes)}:${formatSegment(seconds)}`.split(''));
    return `${formatSegment(days)}:${formatSegment(hours)}:${formatSegment(minutes)}:${formatSegment(seconds)}`.split('')
})

const interval = ref()
onMounted(() => {
    interval.value = setInterval(() => {
        const update: Date = new Date()
        if (update.getSeconds() !== nowDate.value.getSeconds()) {
            nowDate.value = update
        }
    }, 100)
})
onUnmounted(() => {
    clearInterval(interval.value)
})
</script>

<style scoped></style>
