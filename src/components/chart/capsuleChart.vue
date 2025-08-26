<script setup>
import { ref, onMounted, onUnmounted, watch } from "vue";
import * as echarts from 'echarts';

// 图表实例ref
const chartInstance = ref(null);
// 图表容器ref
const capChartRef = ref(null);
// 数据ref
const transactionData = ref([]);

// 模拟后端数据接口
const fetchTransactionData = async () => {
  return new Promise(resolve => {
    setTimeout(() => {
      const fakeData = [
        { name: '台湾证券交易所', value: Math.floor(Math.random() * 2000) + 300 },
        { name: '香港证券交易所', value: Math.floor(Math.random() * 6000) + 1200 },
        { name: '北京证券交易所', value: Math.floor(Math.random() * 3000) + 500 },
        { name: '深圳证券交易所', value: Math.floor(Math.random() * 4000) + 800 },
        { name: '上海证券交易所', value: Math.floor(Math.random() * 5000) + 1000 },
      ];
      resolve(fakeData);
    }, 500);
  });
};

// 初始化图表
const initChart = () => {
  if (capChartRef.value && !chartInstance.value) {
    // 创建图表实例
    chartInstance.value = echarts.init(capChartRef.value);

    // 设置图表配置
    const option = {
      title: {
        text: '证券交易行成交指数',
        left: 'center',
        textStyle: {
          fontSize: 16,
          fontWeight: 'bold',
          color: '#8a6244'
        }
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'none'
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: {
        type: 'value',
        axisLine: { show: false },
        axisTick: { show: false },
        axisLabel: { show: false },
        splitLine: { show: false },
      },
      yAxis: {
        type: 'category',
        data: transactionData.value.map(item => item.name),
        axisLine: { show: false },
        axisTick: { show: false },
        axisLabel: {
          color: '#8a6244',
          fontSize: 15
        }
      },
      series: [{
        name: '成交额',
        type: 'bar',
        data: transactionData.value.map(item => item.value),
        label: {
          show: true,
          position: 'right',
          formatter: '{c}',
          color: '#f8d586',
          fontSize: 15
        },
        itemStyle: {
          // 为不同交易行设置不同颜色
          color: function(params) {
            const colorList = [
              '#ffdc73', // 台湾证券交易所 
              '#ffc67a', // 香港交易所 
              '#ffa07a', // 北京证券交易所
              '#77dd77', //  深圳证券交易所
              '#7aa7ff'  // 上海证券交易所
            ];
            return colorList[params.dataIndex];
          },
          // 设置圆角，实现胶囊效果
          borderRadius: [15, 15, 15, 15]
        },
        barWidth: 15,
        animation: true,
        animationDuration: 1000
      }]
    };

    // 设置图表配置
    chartInstance.value.setOption(option);
  }
};

// 更新图表数据
const updateChartData = (newData) => {
  if (chartInstance.value) {
    chartInstance.value.setOption({
      yAxis: {
        data: newData.map(item => item.name)
      },
      series: [{
        data: newData.map(item => item.value)
      }]
    });
  }
};

// 初始化图表数据
const initChartData = async () => {
  try {
    const data = await fetchTransactionData();
    transactionData.value = data;
    initChart();
  } catch (error) {
    console.error('获取交易数据失败:', error);
  }
};

// 监听数据变化
watch(transactionData, (newData) => {
  if (newData.length > 0) {
    updateChartData(newData);
  }
});

// 处理窗口大小变化
const handleResize = () => {
  if (chartInstance.value) {
    chartInstance.value.resize();
  }
};

onMounted(() => {
  // 确保DOM渲染完成后初始化图表
  setTimeout(() => {
    initChartData();
  }, 100);
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
  // 销毁图表实例
  if (chartInstance.value) {
    chartInstance.value.dispose();
    chartInstance.value = null;
  }
});
</script>

<template>
  <div class="echarts-capsule-chart-container">
    <div ref="capChartRef" style="width: 100%; height: 100%; min-height: 250px;"></div>
  </div>
</template>

<style scoped>
.echarts-capsule-chart-container {
  width: 100%;
  height: 100%;
  padding: 10px;
  box-sizing: border-box;
}
</style>