<script setup>
//import {  } from 'vue'
import { reactive, watch, ref, onMounted, inject, provide } from 'vue'
import CardList from '../components/CardList.vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

const { addToCart, removeFromCart, cart } = inject('cart')
const items = ref([]); // { value: [] }	

const filters = reactive({
	sortBy: 'title',
	searchQuery: ''
});
const onChangeSelect = (event) => {
	filters.sortBy = event.target.value
}
// добавление в корзину
const onClickAddPlus = (item) => {
	if (!item.isAdded) {
		addToCart(item);
	} else {
		removeFromCart(item);
	}
	//console.log(cart);
}
const onChangeSearchInput = debounce((event) => {
	filters.searchQuery = event.target.value
}, 300)
// фильтрация при поиске
const fetchItems = async () => {
	try {
		const params = {
			sortBy: filters.sortBy
		};
		if (filters.searchQuery) {
			params.title = `*${filters.searchQuery}*`;
		}
		const { data } = await axios.get(`https://cd7fd0a3bd89afef.mokky.dev/items`, { params });

		items.value = data.map((obj) => ({ 
		...obj, 
        isFavorite: false,
		isAdded: false,
		favoriteId: null
		}));

		items.value = data
	} catch (err) {
		console.log(err)
	}
}

// проверка API  Favorites
const fetchFavorites = async () => {
	try {
		const { data: favorites } = await axios.get(`https://cd7fd0a3bd89afef.mokky.dev/favorites`);
		items.value = items.value.map(item => {
			const favorite = favorites.find(favorite => favorite.item_id === item.id);

			if (!favorite) {
				return item;
			}
			return { 
				...item, 
				isFavorite: true ,
				favoriteId: favorite.id
			};   
		});
	} catch (err) {
		console.log(err)
	}
}
//========================================================================================================================================================

// добавление / удаление в закладки 
// const addToFavorite = async (item) => {
// 	try {
// 		if (!item.isFavorite) {
// 			const obj = {
// 			item_id: item.id,
// 		}
// 		item.isFavorite = true;
// 		const { data } = await axios.post(`https://cd7fd0a3bd89afef.mokky.dev/favorites`, obj);	
// 		item.favoriteId = data.id
// 		console.log(data);
// 		} else {
// 			item.isFavorite = false;
//             // если уже есть в закладках удаляем
// 			await axios.delete(`https://cd7fd0a3bd89afef.mokky.dev/favorites/${item.favoriteId}`);
// 			item.favoriteId = null;
// 		}
// 	} catch (err) {
// 		console.log(err)
// 	}
// }
//========================================================================================================================================================
// Функция для добавления или удаления из "favorites" в зависимости от условия
const addToFavorite = async (item) => {
    if (!item.isFavorite) {
        await plusToFavorite(item);
    } else {
        await removeFromFavorites(item);
    }
};
// добавление в закладки
const plusToFavorite = async (item) => {
    try {
        const obj = {
            item_id: item.id,
        };
        item.isFavorite = true;
        const { data } = await axios.post(`https://cd7fd0a3bd89afef.mokky.dev/favorites`, obj);
        item.favoriteId = data.id;
        console.log(data);
    } catch (err) {
        console.log(err);
    }
};
// удаление из закладок
const removeFromFavorites = async (item) => {
    try {
        item.isFavorite = false;
        // Если уже есть в "favorites", удаляем
        await axios.delete(`https://cd7fd0a3bd89afef.mokky.dev/favorites/${item.favoriteId}`);
        item.favoriteId = null;
    } catch (err) {
        console.log(err);
    }
	console.log(item.favoriteId);
};
provide('removeFromFavorites', {
	removeFromFavorites
});
//console.log(removeFromFavorites);
//========================================================================================================================================================

// достаём из IP сервера закладки и карточки товаров ?
onMounted( async () => {
	// достаём из localStorage содержимое корзины
	const localCart = localStorage.getItem('cart');
	cart.value = localCart ? JSON.parse(localCart) : [];
	// получаем данные / карточки и закладки 
	await fetchItems();
	await fetchFavorites();
	// проеряем данные / обновляем карточки
	items.value = items.value.map((item) => ({
	...item,
	isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
	}))
})
// следит за корзиной и удаление метки на item
watch(cart, () => {
	items.value = items.value.map((item) => ({
	...item,
	isAdded: false
	}));
});
watch(filters, fetchItems);

</script>

<template>
    <div class="flex justify-between items-center">
			<h2 class="text-3xl font-bold mb-8" >Все кроссовки</h2>

			<div class="flex gap-4">
				<select @change="onChangeSelect" class="border border-slate-200 py-2 px-3 border outline-none rounded-md" id="">
					<option value="name">По названию</option>
					<option value="price">По цене (дешёвые)</option>
					<option value="-price">По цене (дорогие)</option>

				</select>
				<div class="relative">
					<img class="absolute left-4 top-3" src="/search.svg" alt="search">
					<input
					@input="onChangeSearchInput" 
					 class="border border-slate-200 py-2 pl-10 rounded-md pr-2 outline-none focus:border-slate-400 transition ease duration-300" placeholder="Поиск" type="text">
				</div>
			</div>

		</div>
		<div class="mt-10">
			<CardList
			:items="items"
			@add-to-favorite ="addToFavorite"
			@add-to-cart="onClickAddPlus"

			
			

			/>
		</div>
</template>



<style>

</style>