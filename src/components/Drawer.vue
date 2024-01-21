<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartItemList from "./CartItemList.vue";
import InfoBlock from "./InfoBlock.vue";
import { ref, computed, inject } from "vue";
import axios from "axios";
//const { closeDrawer } = inject('cart');
//const emit = defineEmits(['createOrder']);

const isCreating = ref(false);
const orderId = ref(null);
const { cart, closeDrawer } = inject('cart');
const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
    
})

// отправка в order и очистка корзины 
const createOrder = async () => {
	try {
		isCreating.value = true;
		const { data } = await axios.post('https://cd7fd0a3bd89afef.mokky.dev/orders', {
			items: cart.value,
			totalPrice: props.totalPrice.value,
		});
		//console.log(data);
		cart.value = [];
        orderId.value = data.id;
		return data;
	} catch (err) {
		console.log(err)
	} finally {
		isCreating.value = false;
	}
}
const cardEmpty = computed(() => {
	return cart.value.length === 0;
})
const buttonDisabled = computed(() => isCreating.value || cardEmpty.value);
</script>
<template>
    <div @click="closeDrawer" class="fixed top-0 left-0 bottom-0 w-full h-full bg-black bg-opacity-70 z=10" ></div> 
        <div class="bg-white w-96 fixed top-0 bottom-0 right-0 z-20 p-8 cart">
            <DrawerHead/> 
            <div class="grid content-between cart__content relative">
                
                <div v-if="!totalPrice || orderId" class="flex h-full items-center">
                    <InfoBlock
                    v-if="!totalPrice && !orderId"
                    title="Корзина пуста" 
                    description="Добавьте хотя бы одну пару кроссовок, чтобы оформить заказ."
                    imageUrl="/package-icon.png"
                />
                
                
                    <InfoBlock
                    v-if="orderId"
                    title="Заказ оформлен!" 
                    :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке.`"
                    imageUrl="/order-success-icon.png"
                />
                </div>
                <div v-else>
                    <CartItemList /> 

                <div  class="flex flex-col gap-4 mt-7 justify-end cart__footer">
                    <div class="flex gap-2 ">
                        <span>Итого:</span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ totalPrice }} руб.</b>
                    </div>
                    <div class="flex gap-2 ">
                        <span>Налог 5%:</span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ vatPrice }} руб.</b>
                    </div>
            <button
                    :disabled="buttonDisabled" 
                    @click="createOrder"
                    class="bg-lime-500 mt-4 w-full text-white disabled:bg-slate-300 cursor-pointer py-3 rounded-xl hover:bg-lime-600 transition ease duration-300 active:bg-lime-700">
                    Оформить заказ
            </button>
                </div>
                </div>
            
            </div>
        </div>
</template>

<style lang="scss" scoped>
.cart {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    &__content {
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }
    &__footer {
        position: absolute;
        bottom: 0;
        width: 100%;
    }
}

</style>