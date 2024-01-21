<script setup>
defineProps({
	totalPrice: Number
})

	
	const emit = defineEmits(['openDrawer']);
</script>
<template>
    <header class="flex justify-between border-b border-slate-300 px-8 py-6  ">
		<router-link to="/">
		<div class="flex items-center gap-4" >
		<img src="/logo.png" alt="logo" class="w-10" />
	
		<div class="flex flex-col justify-center" >
			<h2 class="text-xl font-bold uppercase m-0 " >Vue Sneakers</h2>
			<p class="opacity-70 mb-0" >Магазин красовок</p>
		</div>
	</div>
		</router-link>
	

	<ul class="flex items-center gap-10 " >
		<li 
		@click="() => emit('openDrawer')" 
		class="flex items-center gap-3  cursor-pointer">
			<img src="/cart.svg" alt="cart">
			<b class="opacity-70 hover:opacity-100 transition ease duration-300 " >{{ totalPrice }} руб</b>
		</li>
		<router-link to="/favorites">
		<li class="flex items-center gap-3 cursor-pointer">
			<img src="/heart.svg" alt="heart">
			<b class="opacity-70 hover:opacity-100 transition ease duration-300" >Закладки</b>
		</li>
		</router-link>
		
		<li class="flex items-center gap-3 cursor-pointer ">
			<img src="/profile.svg" alt="profile">
			<b class="opacity-70 hover:opacity-100 transition ease duration-300" >Профиль</b>
		</li>

	</ul>
</header>
</template>

<style scoped>

</style>
