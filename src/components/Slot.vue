<script setup>
import {ref} from 'vue'
</script>
<template>
    <div> 下边是默认插槽,内容是父组件传过来的:</div>
    <slot></slot>
    <div> 下边是slot name为 red的插槽,内容是父组件传过来的:</div>
    <slot name="red"> </slot>
</template>