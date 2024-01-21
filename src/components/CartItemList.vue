<script setup>
import { inject } from "vue";
import CartItem from './CartItem.vue'
const { cart, removeFromCart } = inject('cart');



</script>

<template>

    <div class="flex flex-col flex-1 justify-between gap-4 " v-auto-animate>
		<CartItem 
			v-for="item in cart"
			:key="item.id"
			:id="item.id"
			:title="item.title"
			:imageUrl="item.imageUrl"
			:price="item.price"
			:isAdded="item.isAdded"
			:isFavorite="item.isFavorite"	
			@on-click-remove="removeFromCart(item)"

			
			
			
			
			/>
		
		
	</div>

</template>