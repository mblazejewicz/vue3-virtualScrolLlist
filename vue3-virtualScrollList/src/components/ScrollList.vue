<script setup>
import { ref } from 'vue'
import { useVirtualList, useInfiniteScroll } from '@vueuse/core'

const data = ref(Array.from(Array(5000).keys(), () => 'lorem Ipsum'))

const { list, containerProps, wrapperProps } = useVirtualList(data, { itemHeight: 96, overscan: 10 })

useInfiniteScroll(containerProps.ref,
    () => {
        //load more data
        data.value.push(...Array.from(Array(20).keys(), () => 'More data loaded Item'))
    },
    { distance: 10 }) //distance to bottom from current in pixels
</script>

<template>
    <div class="flex flex-col px-5 w-[500px]">
    <h2 class="rounded m-5 bg-neutral-500 p-2 text-center">Total: {{ data.length }}</h2>
    <div v-bind="containerProps" class="h-[500px] p-2 rounded ">
        <div v-bind="wrapperProps" class="max-w-sm mx-auto">
            <div v-for="{ index, data } in list" :key="index" class="rounded-lg h-[80px] flex flex-col px-4 justify-center bg-neutral-800 mb-4 border-neutral-600 text-cyan-100">
                <h2 class="mb-2 text-2xl"> Item #{{ index }}</h2>
                <p class="text-sm"> {{ data }}</p>
            </div>
        </div>
    </div>
</div>
</template>