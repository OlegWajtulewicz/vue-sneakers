<script setup>
import { computed,  provide, ref, watch } from 'vue'
//import axios from 'axios'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'


/* Корзина START */
const cart = ref([]);
const drawerOpen = ref(false);

const totalPrice = computed(() => {
	return cart.value.reduce((acc, item) => acc + item.price, 0);
})
const vatPrice = computed(() => {
	return Math.round(((totalPrice.value * 5) / 100));
})


 

const closeDrawer = () => {
	drawerOpen.value = false
}
const openDrawer = () => {
	drawerOpen.value = true
}
// добавление в корзину
const addToCart = (item) => {
	cart.value.push(item);
	item.isAdded = true
}
//  удаление из корзины
const removeFromCart = (item) => {
	cart.value.splice(cart.value.indexOf(item), 1)
	item.isAdded = false
}

// следит за карзиной и сохраняет в localStorage шзменения и делает глубокую проверку
watch(cart, () => {
	localStorage.setItem('cart', JSON.stringify(cart.value));
},
{
	deep: true
});

provide('cart', {
	cart,
	closeDrawer,
	openDrawer,
	addToCart,
	removeFromCart,
});
/* Корзина END */
</script>


<template>
	<div class="w-4/5  m-auto bg-white rounded-xl shadow-xl mt-14 mb-14">
	<Header 
		:total-price="totalPrice"
		@open-drawer="openDrawer"
		/>

	<div class="p-10">
		<router-view></router-view>
	</div>
	<Drawer
		v-if="drawerOpen"
		:total-price="totalPrice"
		:vat-price="vatPrice"
		@create-order="createOrder"
	/>
	</div> 
</template>




<style lang="scss" scoped>

</style>













