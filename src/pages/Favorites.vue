<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import CardList from "../components/CardList.vue";
// храним список закладок
const favorites = ref([]);

const loading = ref(false);
const error = ref(null);

// const removeFromFavorites = async (item) => {
//     try {
//         item.isFavorite = false;
//         // Если уже есть в "favorites", удаляем
//         await axios.delete(`https://cd7fd0a3bd89afef.mokky.dev/favorites/${item.favoriteId}`);
//         item.favoriteId = null;
//     } catch (err) {
//         console.log(err);
//     }
// 	console.log(item.favoriteId);
// };
// provide('removeFromFavorites', {
// 	removeFromFavorites
// });
//console.log(removeFromFavorites);

// запрос на сервер о закладках
// onMounted(async () => {
//     try {
//       const { data } = await axios.get("https://cd7fd0a3bd89afef.mokky.dev/favorites?_relations=items,users"); 
//       favorites.value = data.map((obj) => obj.item);
//     } catch (err) {
//       console.log(err);
//     }
// })

onMounted(async () => {
  try {
    loading.value = true;
    const { data } = await axios.get("https://cd7fd0a3bd89afef.mokky.dev/favorites?_relations=items,users");
    favorites.value = data.map((obj) => obj.item);
  } catch (err) {
    console.error(err);
    error.value = "Произошла ошибка при загрузке данных.";
  } finally {
    loading.value = false;
  }
});



</script>

<template>
  <h2 class="text-3xl font-bold mb-8" >Мои закладки</h2>

  <CardList
        :items="favorites"
        is-favorites
        


        />
        <div v-if="loading">loading...</div>
        <div v-if="error">{{ error }}</div>
</template>



<style>

</style>