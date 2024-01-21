<script setup>
import { defineProps } from 'vue';


defineProps({
		id: Number,
        imageUrl: String,
        title: String,
        price: Number,
		isAdded: Boolean,
		isFavorite: Boolean,
		onClickLike: Function,
		onClickAdd: Function,
		showCloseButton: Boolean,
    });

const emit = defineEmits([ 'onRemoveFavorite' ]);


</script>

<template>
  	
<div>
	<div class="relative z-2 bg-white border border-slate-100 rounded-xl p-8 cursor-pointer transition hover:shadow-xl hover:transform hover:-translate-y-1"  >
		
        <img v-if="onClickLike" 
			@click="onClickLike" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="like" class="absolute top-8 left-8">
		<img :src="imageUrl" alt="sneakers">
		<p class="font-bold mt-5"> {{ title }} </p>

		<div class="flex justify-between mt-5">
			<div class="flex flex-col gap-2" >
				<span class="text-slate-400">Цена:</span>
				<b> {{ price }} руб.</b>
			</div>
			<img v-if="onClickAdd" 
				@click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="plus">
			<img v-if="showCloseButton" 
				@click="emit('onRemoveFavorite')" 	
				src="/close.svg" alt="close">
		</div> 

	</div>
</div>

</template>

