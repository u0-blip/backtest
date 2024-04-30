<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import * as echarts from 'echarts';
import type { ECharts } from 'echarts';

// Define props with TypeScript interface
const props = defineProps<{
  className?: string,
  width?: string,
  height?: string,
  autoResize: boolean,
  chartData: { expectedData: number[], actualData: number[] }
}>();

// Providing default values for props
const className = props.className ?? 'chart';
const width = props.width ?? '100%';
const height = props.height ?? '350px';

// Reactive chart instance
const chart = ref<ECharts | null>(null);

// Initialize chart
function initChart(el: HTMLElement) {
  chart.value = echarts.init(el, 'macarons');
  setOptions(props.chartData);
}

// Set chart options
function setOptions({ expectedData, actualData }: { expectedData: number[], actualData: number[] }) {
  if (!chart.value) return;
  chart.value.setOption({
    xAxis: {
      data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      boundaryGap: false,
      axisTick: { show: false }
    },
    grid: {
      left: 10, right: 10, bottom: 20, top: 30, containLabel: true
    },
    tooltip: {
      trigger: 'axis',
      axisPointer: { type: 'cross' },
      padding: [5, 10]
    },
    yAxis: { axisTick: { show: false } },
    legend: { data: ['expected', 'actual'] },
    series: [{
      name: 'expected',
      itemStyle: { normal: { color: '#FF005A', lineStyle: { color: '#FF005A', width: 2 } } },
      smooth: true, type: 'line', data: expectedData,
      animationDuration: 2800, animationEasing: 'cubicInOut'
    }, {
      name: 'actual',
      smooth: true, type: 'line',
      itemStyle: { normal: { color: '#3888fa', lineStyle: { color: '#3888fa', width: 2 }, areaStyle: { color: '#f3f8ff' } } },
      data: actualData,
      animationDuration: 2800, animationEasing: 'quadraticOut'
    }]
  });
}

// Watch for changes in chartData
watch(() => props.chartData, (newVal) => {
  setOptions(newVal);
}, { deep: true });

// Lifecycle hooks for mounting and unmounting
onMounted(() => {
  initChart(document.querySelector('.chart') as HTMLElement); // This assumes your component's root element is accessible this way
});

onBeforeUnmount(() => {
  if (chart.value) {
    chart.value.dispose();
    chart.value = null;
  }
});

</script>
