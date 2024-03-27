<script setup>
const [title, modifiers] = defineModel('title', {
    default: '666',
    set: (value) => {
        if(modifiers.trimAll) {
            value = value.replace(/\s/g, '')
        }
        return value
    }
});
// <TestVModel v-model:title.trimAll="text"></TestVModel>

</script>

<template>
    <input type="text" v-model="title">
</template>
