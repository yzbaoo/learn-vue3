<script setup>
import {ref} from 'vue'

const props = defineProps({
    initialCount: {
        type: Number,
        default: 0
    }
})
const emit = defineEmits({
    incrementCb: null
})

const count = ref(props.initialCount)
const increment = () => {
    count.value++
    emit('incrementCb', count.value, 1, 2)
}
</script>

<template>
    <div>
        <h1>Count: {{ count }}</h1>
        <button @click="increment">Increment</button>
    </div>
</template>