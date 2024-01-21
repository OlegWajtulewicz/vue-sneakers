<script setup>
defineProps({
	title: String,
	price: Number,
	isAdded: Boolean
})




</script>

<template>
  	<div class="product">
	  	<h2> Товар: {{ title }} </h2>
	  	<h3> Цена: {{ price }} </h3>
		<h3> Добавлен: {{ isAdded }} </h3>
		<slot></slot>
	</div>
</template>

<style lang="scss" scoped>
.product {
	border: 2px solid blueviolet;
	padding: 10px;
	margin-bottom: 10px;
}

</style>
