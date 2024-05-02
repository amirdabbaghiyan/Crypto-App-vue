<template>
  <tr @click="showHandler">
    <td>
      <div class="symbol">
        <img :src="props.coin.image" alt="" />
        <span>{{ props.coin.symbol.toUpperCase() }}</span>
      </div>
    </td>
    <td>{{ props.coin.name }}</td>
    <td>
      \${{ props.coin.current_price.toLocaleString() }}
    </td>
    <td :class="`{ ${props.coin.price_change_percentage_24h} > 0 ? success : error }`" >
      {{ props.coin.price_change_percentage_24h.toFixed(2) }}%
    </td>
    <td>{{ props.coin.total_volume.toLocaleString() }}</td>
    <td>
      <!-- <img :src="`${props.coin.price_change_percentage_24h} > 0 ? ${chartUp} : ${chartDown}`" :alt="props.coin.name" /> -->
      <!-- <div v-if="`${props.coin.price_change_percentage_24h.toFixed(2)} < 0 `">
        <img src="../assets/images/chart-up.png" :alt="props.coin.name" />
      </div>
      <div v-else>
        <img src="../assets/images/chart-down.png" :alt="props.coin.name" />
      </div> -->
    </td>
  </tr>
</template>

<script setup>
import { defineProps, ref } from 'vue';
import chartUp from '@/assets/images/chart-up.svg';
import chartDown from '@/assets/images/chart-down.svg';
import { marketChart } from '@/server/api';

const amir = ref();

const props = defineProps({
  coin: Object,
  setChart: Function
});

const showHandler = async () => {
  try {
    const res = await fetch(marketChart(props.coin.id));
    const json = await res.json();
    props.setChart({ ...json, coin: props.coin });
    if (props.coin.price_change_percentage_24h > 0){
      amir.value = chartUp;
    }else{
      amir.value = chartDown;
    }
  } catch (error) {
    props.setChart(null);
  }
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