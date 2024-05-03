<template>
    <div class="container">
        <PulseLoader v-if="isLoading" :loading="loading" color="var(--primary-color)" size="8px"/>

        <table
        v-else:
        class="table">
            <thead>
                <tr>
                    <th>Coin</th>
                    <th>Name</th>
                    <th>Price</th>
                    <th>24h</th>
                    <th>Total Volume</th>
                    <th>Chart</th>
                </tr>
            </thead>
            <tbody>
                <TableRow
                v-for="coin in props.coins"
                :key="coin.id"
                :coin="coin"
                @chart="setChart" />
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import TableRow from './TableRow.vue';

const props = defineProps(['isLoading', 'coins']);

const emit = defineEmits(['chart']);
const setChart = (chart) => {
  emit('chart', chart);
};
</script>

<style>
.container {
    display: flex;
    justify-content: center;
    margin: 50px 0 100px;
    min-height: 1000px;
}

.spinner {
    display: flex;
    align-items: center;
    justify-content: center;
}

.table {
    width: 100%;
    border-collapse: collapse;
}

.table thead {
    border-bottom: 2px solid var(--text-color);
    margin-bottom: 20px;
}

.table th {
    font-size: 1.2rem;
    text-align: left;
    padding: 10px 0;
}

.table tbody tr {
    height: 80px;
    border-bottom: 1px solid var(--color-border);
    font-weight: 600;
    font-size: 1.1rem;
    cursor: pointer;
}

.symbol {
    display: flex;
    align-items: center;
    cursor: pointer;
}

.symbol img {
    width: 25px;
    height: 25px;
    margin-right: 10px;
}

.symbol span {
    font-weight: 600;
    font-size: 1.2rem;
    color: #9fa6b7;
}

.success {
    color: #57bc7c;
}

.error {
    color: #d33636;
}

@media (max-width: 600px) {
    .table {
        display: block;
        overflow-y: scroll;
    }

    .table th,
    .table td {
        padding: 0 10px;
    }

}
</style>