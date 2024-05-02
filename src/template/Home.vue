<template>
    <main id="main">
        <Search
        :currency="currency"
        @update:currency="setCurrency"
        />

        <TableCoin
        :coins="coins"
        :is-loading="isLoading"
        @update:chart="setChart"
        />
        
        <Pagination
        :page="page"
        @update:page="setPage"
        />

        <!-- <Chart
        v-if="chart"
        :chart="chart" @update:chart="setChart"
        /> -->
    </main>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
// // components
import TableCoin from '@/components/TableCoin';
import Pagination from '@/components/Pagination';
// import Chart from '@/parts/Chart/Chart.vue';
import Search from '@/components/Search';
import { getCoinList } from '@/server/api';

const coins = ref([]);
const isLoading = ref(true);
const page = ref();
const currency = ref('usd');
const chart = ref(null);

const setChart = (newChart) => {
    chart.value = newChart;
};

const setPage = (newPage) => {
    page.value = newPage;
};

const getData = async () => {
    try {
        const res = await fetch(getCoinList(page.value, currency.value));
        const json = await res.json();
        coins.value = json;
        console.log(json[0]);
        isLoading.value = false;
    } catch (error) {
        alert('this is error');
    }
};

onMounted(getData);

watch([page, currency], () => {
  isLoading.value = true;
  getData();
});
</script>