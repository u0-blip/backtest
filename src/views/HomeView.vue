<script setup lang="ts">
import LineChart from '../components/LineChart.vue';

const lineChartData = {
  expectedData: [820, 932, 901, 934, 1290, 1330, 1320],
  actualData: [820, 932, 901, 934, 1290, 1330, 1320]
};
</script>

<template>
  <main>
    <LineChart :chart-data="lineChartData" :autoResize="true" />
  </main>
</template>
