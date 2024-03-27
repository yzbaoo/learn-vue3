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
const handleButtonClick = () => {
    console.error('原生button被点击了');
}
</script>
<template>
    <button @click="handleButtonClick">事件透传</button>
</template>