<script setup>
import {ref} from 'vue'
const props = defineProps({
    color: {
        type: String,
        default: 'red'
    }
})
</script>

<template>
    <div :style="{color: color}">这是一个loading</div>
</template>