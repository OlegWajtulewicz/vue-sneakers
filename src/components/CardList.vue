<script setup>
import Card from './Card.vue'
import { defineProps,  defineEmits, inject, onMounted } from 'vue';


 defineProps ({
    items: Array,
    isFavorites: Boolean,
    addToFavorite: Function,
    //removeFromFavorites: Function
});

const removeFromFavorites = inject('removeFromFavorites');
console.log(removeFromFavorites);

const emit = defineEmits(['addToFavorite', 'addToCart', 'removeFromFavorites']); 


onMounted(() => {
  if (!removeFromFavorites) {
    console.error('removeFromFavorites is not defined');
  }
});


</script>

<template>

    <div class="grid grid-cols-4 gap-5 " v-auto-animate>
		<Card 
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title" 
            :imageUrl="item.imageUrl"
            :price="item.price"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"
            :onClickLike="isFavorites ? null : () => emit('addToFavorite', item)"
            :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
            :showCloseButton="isFavorites"

            @on-remove-favorite="() => removeFromFavorites(item)"
            />
	</div>

</template>



