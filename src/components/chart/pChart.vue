<script setup>
import * as echarts from 'echarts';
import { ref, onMounted, onUnmounted } from 'vue';

const pChartRef = ref(null);
let myChart = null;

const option = {
  tooltip: { trigger: 'item' },
  legend: { top: '5%', left: 'center' },
  
  series: [
    {
      name: '行业占比',
      type: 'pie',
      radius: ['40%', '70%'],
      avoidLabelOverlap: false,
      padAngle: 5,
      itemStyle: { borderRadius: 5 },
      label: { show: false, position: 'center' },
      emphasis: {
        label: { show: true, fontSize: 16, fontWeight: 'bold', }
      },
      labelLine: { show: false },
    //   roseType:'area',
      data: [
        { value: 35, name: '科技' },
         { value: 15, name: '医药' },
        { value: 20, name: '消费' },
        { value: 12, name: '金融' },
        { value: 18, name: '其他' }
      ]
    }
  ]
};

// 初始化图表
const initChart = () => {
  if (pChartRef.value && !myChart) {
    myChart = echarts.init(pChartRef.value);
    myChart.setOption(option);
  }
};

// 响应式调整
const handleResize = () => {
  myChart?.resize();
};

onMounted(() => {
  initChart();
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
  myChart?.dispose();
  myChart = null;
});
</script>

<template>
  <div class="p-chart">
    <div ref="pChartRef" style="width: 100%; height: 100%;min-height:230px;"></div>
  </div>
</template>

<style scoped>
.p-chart {
  width: 100%;
  height: 100%;
  padding: 10px;
  box-sizing: border-box;
}
</style>