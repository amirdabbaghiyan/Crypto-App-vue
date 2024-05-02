<template>
  <div :class="container">
    <span
    :class="cross"
    @click="closeChart">X</span>
    <div :class="chart">
      <div :class="name">
        <img :src="chart.coin.image" alt="" />
        <p>{{ chart.coin.name }}</p>
      </div>

      <!-- <div :class="graph">
        <ChartComponent
        :data="convertedData"
        :type="type" />
      </div> -->

      <div
      :class="types"
      @click="typeHandler">
        <button :class="{ selected : type === 'prices' }">Prices</button>
        <button :class="{ selected : type === 'market_caps' }">Market Cap</button>
        <button :class="{ selected : type === 'total_volumes' }">Total Volume</button>
      </div>

      <div :class="details">
        <div>
          <p>Prices:</p>
          <span>${{ chart.coin.current_price }}</span>
        </div>
        <div>
          <p>Ath:</p>
          <span>${{ chart.coin.ath }}</span>
        </div>
        <div>
          <p>Market Cap:</p>
          <span>${{ chart.coin.market_cap }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { convertData } from '../../helpers/convertData';
// import ChartComponent from './ChartComponent.vue';

// const props = defineProps({
//   chart: Object,
//   setChart: Function
// });

const type = ref('prices');

const closeChart = () => {
  props.setChart(null);
};

const typeHandler = (event) => {
  if (event.target.tagName === 'BUTTON') {
    type.value = event.target.innerText.toLowerCase().replace(' ', '_');
  }
};

// const convertedData = computed(() => convertData(props.chart, type.value));
</script>

<style>
.container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(3px);
}

.cross {
  display: inline-block;
  font-size: 1.5rem;
  font-weight: 700;
  background-color: #d33636;
  color: var(--text-color);
  width: 30px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  margin: 30px 0 0 30px;
  border-radius: 5px;
  cursor: pointer;
}

.chart {
  width: 800px;
  margin: auto;
  padding: 30px 20px;
  margin-top: 20px;
  background-color: var(--bg-color);
  border: 2px solid #404042;
  border-radius: 20px;
}

.graph {
  width: 760px;
  height: 300px;
}

.name {
  display: flex;
  align-items: center;
  margin: 0 10px 30px;
}

.name img {
  width: 40px;
  height: 40px;
  margin-right: 20px;
}

.name p {
  font-size: 1.5rem;
  font-weight: 700;
}

.types {
  margin-top: 20px;
}

.types button {
  margin: 10px 20px;
  background-color: #18181cdb;
  border: 1px solid var(--primary-color);
  color: var(--primary-color);
  font-size: 1rem;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.types .selected {
  background-color: var(--primary-color);
  color: var(--text-color);
}

.details {
  display: flex;
  justify-content: space-between;
  margin: 20px 20px 0;
}

.details>div {
  display: flex;
  font-size: 1.1rem;
}

.details p {
  margin-right: 7px;
  color: var(--primary-color);
  font-weight: 700;
}
</style>
