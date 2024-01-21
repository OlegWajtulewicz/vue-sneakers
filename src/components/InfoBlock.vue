<script setup>
    defineProps({
        title: String,
        imageUrl: String,
        description: String,
    })
</script>

<template>
    <div  class="flex flex-col items-center text-center gap-4 w-72 mx-auto">
        <img height="70" width="70"
            :src="imageUrl" alt="info image">
        <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
        <p class="text-slate-400">{{ description }}</p>
    </div>
  
</template>


