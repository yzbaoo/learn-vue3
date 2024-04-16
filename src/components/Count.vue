<script setup>
import {ref, watchEffect} from 'vue'

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
// 1. ❌count变化时并不会导致doubleCount更新
// const doubleCount = ref(count.value * 2)

// 2. ✅ doubleCount会按预期更新
// const doubleCount = ref(0)
// watchEffect(() => {
//     doubleCount.value = count.value * 2
// })

// 3. ❌count变化,useDouble不会重复执行,所以内部watchEffect也是徒劳
// const useDouble = (count) => {
//     const doubleCount = ref(count * 2)
//     watchEffect(() => {
//         doubleCount.value = count * 2
//     })
//     return doubleCount
// }
// const doubleCount = useDouble(count.value)

// 4. ✅如何改造呢? 试试直接将count传递进入useDouble试试
const useDouble = (count) => {
    const doubleCount = ref(count.value * 2)
    watchEffect(() => {
        doubleCount.value = count.value * 2
    })
    return doubleCount
}
const doubleCount = useDouble(count)


const increment = () => {
    count.value++
    emit('incrementCb', count.value, 1, 2)
}


</script>

<template>
    <div>
        <h1>Count: {{ count }}</h1>
        <h1>DoubleCount: {{ doubleCount }}</h1>
        <button @click="increment">Increment</button>
    </div>
</template>