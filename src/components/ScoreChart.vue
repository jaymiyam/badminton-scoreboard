<template>
  <canvas ref="ChartLine"></canvas>
</template>

<script setup>
import { Chart } from 'chart.js';
import { ref, onMounted, toRaw } from 'vue';

const { scoringHistory } = defineProps({
  scoringHistory: {
    type: Array,
    required: true,
  },
});
const ChartLine = ref(null);
let chart;

onMounted(() => {
  const canvas = ChartLine.value;
  chart = new Chart(canvas, {
    type: 'line',
    data: toRaw(chartData),
    options: options,
  });
});

const chartData = ref({
  labels: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
  datasets: [
    {
      label: 'Left Team',
      backgroundColor: '#f87979',
      data: [0, 0, 0, 0],
    },
    {
      label: 'Right Team',
      backgroundColor: '#337979',
      data: [1, 2, 3, 4],
    },
  ],
});

const options = {
  responsive: true,
  maintainAspectRatio: false,
};

console.log(scoringHistory);
</script>

<style lang="scss" scoped></style>
