<template>
    <div class="searchBox">
        <input
        type="text"
        placeholder="Search"
        v-model="text"/>

        <select v-model="currency" @click="sendCurrency">
            <option value="usd">USD</option>
            <option value="eur">EUR</option>
            <option value="jpy">JPY</option>
        </select>

        <div v-if="coins.length || isLoading"
        class="searchResult">
            <PulseLoader v-if="isLoading" :loading="loading" color="var(--primary-color)" size="8px"/>

            <ul v-else>
                <li 
                v-for="coin in coins"
                :key="coin.id">
                    <img
                    :src="coin.thumb"
                    :alt="coin.name" />
                    <p>{{ coin.name }}</p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { ref, watch, defineEmits } from 'vue';
import { searchCoin } from '@/server/api.js';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';

const currency = ref(localStorage.getItem("currency") || 'usd');
const text = ref('');
const coins = ref([]);
const isLoading = ref(false);
localStorage.setItem("currency" , currency.value);

const fetchApi = async () => {
    coins.value = [];
    if (!text.value) {
        isLoading.value = false;
        return;
    }
    try {
        isLoading.value = true;
        const res = await fetch(searchCoin(text.value));
        const json = await res.json();
        if (json.coins) {
            coins.value = json.coins;
            isLoading.value = false;
        } else {
            alert(json.status.error_message);
        }
    } catch (error) {
        if (error.name !== 'AbortError') {
            alert(error.message);
        }
    }
};
const emit = defineEmits(['currency']);
const sendCurrency = () => {
    emit('currency', currency.value);
}
     
watch(text, fetchApi);
watch(currency, () => {
    localStorage.setItem( "currency" , currency.value );
});
</script>

<style>
.searchBox {
    margin-top: 50px;
    position: relative;
}

.searchBox input {
    width: 300px;
    height: 50px;
    padding: 10px;
    font-size: 1.1rem;
    color: var(--text-color);
    background-color: var(--secondary-color);
    border: none;
    border-radius: 5px;
}

.searchBox select:focus {
    outline: none;
}

.searchBox select {
    background-color: var(--secondary-color);
    height: 50px;
    border: none;
    border-radius: 5px;
    margin-left: 20px;
    color: var(--text-color);
    padding: 0 10px;
    font-size: 1.1rem;
}

.searchResult {
    position: absolute;
    text-align: center;
    top: 60px;
    width: 300px;
    height: 400px;
    border-radius: 5px;
    overflow-y: scroll;
    background-color: #18181c;
    border: 1px solid var(--color-border);
    padding: 20px;
}

::-webkit-scrollbar {
    width: 0px;
    background-color: transparent;
}

.searchResult li {
    list-style: none;
    display: flex;
    margin-bottom: 15px;
    padding-bottom: 5px;
    border-bottom: 2px solid var(--color-border);
}

.searchResult li img {
    margin-right: 10px;
    width: 25px;
    height: 25px;
}
</style>