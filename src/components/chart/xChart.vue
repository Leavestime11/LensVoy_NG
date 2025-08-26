<script setup>
import * as echarts from 'echarts';
import { ref, onMounted, onUnmounted } from 'vue';

const xChartRef = ref(null);
const colors = ['#5470C6', '#EE6666'];
let myChart = null;

const option = {
  color: colors,
  tooltip: {
    trigger: 'none',
    axisPointer: {
      type: 'cross'
    }
  },
  title: {
    text: '日内交易图表 (包含午休断点)',
    left: 'center',
    textStyle: {
      fontSize: 16,
      fontWeight: 'bold',
      color: '#8a6244'
    }
  },
  legend: {},
  grid: {
    top: 70,
    bottom: 50
  },
  xAxis: [
    {
      type: 'category',
      axisTick: {
        alignWithLabel: true
      },
      axisLine: {
        onZero: false,
        lineStyle: {
          color: colors[1]
        }
      },
      axisPointer: {
        label: {
          formatter: function (params) {
            return (
              'Precipitation  ' +
              params.value +
              (params.seriesData.length ? '：' + params.seriesData[0].data : '')
            );
          }
        }
      },
      // prettier-ignore
      data: ['2016-1', '2016-2', '2016-3', '2016-4', '2016-5', '2016-6', '2016-7', '2016-8', '2016-9', '2016-10', '2016-11', '2016-12']
    },
    {
      type: 'category',
      axisTick: {
        alignWithLabel: true
      },
      axisLine: {
        onZero: false,
        lineStyle: {
          color: colors[0]
        }
      },
      axisPointer: {
        label: {
          formatter: function (params) {
            return (
              'Precipitation  ' +
              params.value +
              (params.seriesData.length ? '：' + params.seriesData[0].data : '')
            );
          }
        }
      },
      // prettier-ignore
      data: ['2015-1', '2015-2', '2015-3', '2015-4', '2015-5', '2015-6', '2015-7', '2015-8', '2015-9', '2015-10', '2015-11', '2015-12']
    }
  ],
  yAxis: [
    {
      type: 'value'
    }
  ],
  series: [
    {
      name: 'Precipitation(2015)',
      type: 'line',
      xAxisIndex: 1,
      smooth: true,
      emphasis: {
        focus: 'series'
      },
      data: [
        2.6, 5.9, 9.0, 26.4, 28.7, 70.7, 175.6, 182.2, 48.7, 18.8, 6.0, 2.3
      ]
    },
    {
      name: 'Precipitation(2016)',
      type: 'line',
      smooth: true,
      emphasis: {
        focus: 'series'
      },
      data: [
        3.9, 5.9, 11.1, 18.7, 48.3, 69.2, 231.6, 46.6, 55.4, 18.4, 10.3, 0.7
      ]
    }
  ]
};

/**
 * 初始化图表
 */
function initChart() {
  if (xChartRef.value && !myChart) {
    // 创建图表实例
    myChart = echarts.init(xChartRef.value);
    // 设置图表配置
    myChart.setOption(option);
    console.log('图表初始化成功');
  }
}

/**
 * 处理窗口大小变化
 */
function handleResize() {
  if (myChart) {
    myChart.resize();
  }
}

// 组件挂载时初始化图表
onMounted(() => {
  // 添加延迟确保DOM完全渲染
  setTimeout(() => {
    initChart();
  }, 100);
  window.addEventListener('resize', handleResize);
});

// 组件卸载时销毁图表
onUnmounted(() => {
  if (myChart) {
    myChart.dispose();
    myChart = null;
  }
  window.removeEventListener('resize', handleResize);
});
</script>

<template>
  <div class="x-chart">
    <div ref="xChartRef" style="width: 100%; height: 100%; min-height: 200px;"></div>
  </div>
</template>

<style scoped>
.x-chart {
  width: 100%;
  height: 100%;
  padding: 10px;
  box-sizing: border-box;
}
</style>