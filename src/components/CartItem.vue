<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,

})

</script>
<template>
    <div class=" flex items-center gap-4 w-100 border border-slate-200 p-4 rounded-xl py-4 ">
        <img class="w-16 h-16" :src="imageUrl" :alt="title">
    <div class="flex flex-col flex-1">
        <p class="mb-1 flex-1"> {{ title }} </p>
        <div class="flex justify-between mt-2">
            <b class="flex-1">{{ price }} руб.</b>
            <img 
            @click="emit('onClickRemove')" 
            class="opacity-40 hover:opacity-100 transition ease duration-300 cursor-pointer" 
            src="/close.svg" alt="close">
        </div>
    </div>
    </div>
</template>