<script setup>
import { ref, onMounted, onUnmounted, reactive } from "vue";
import * as echarts from 'echarts';
import chinaMap from '@/assets/jason/china.json';

const MapRef = ref(null);
let myChart = null;

// 添加股票成交数据
const stockData = reactive({
  shanghai: '32,456.78亿',
  shenzhen: '28,912.34亿',
  chuangye: '15,678.90亿'
});

onMounted(() => {
  chartInit();
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
  if (myChart) {
    myChart.dispose();
    myChart = null;
  }
});

const chartInit = () => {
  if (!MapRef.value) return;

  myChart = echarts.init(MapRef.value);
  echarts.registerMap('china', chinaMap);

  const option = {
    tooltip: {},
    geo: {
      type: 'map',
      map: 'china',
      roam: true, // 允许缩放和平移
      itemStyle: {
        areaColor: '#ffede4',
        borderColor: '#942f00'
      },
      emphasis: {
        itemStyle: {
          areaColor: '#ffb669',
          color:'#fff',
        },
        label: {
          show: false,
        }
      }
    },
    series: [
      {
        type: 'effectScatter',
        coordinateSystem: 'geo',
        data: [
          {
            name: '北京证券交易所',
            value: [116.36, 39.9],
            itemStyle: { color: '#ff4500' },
          },
          {
            name: '上海证券交易所',
            value: [121.50, 31.23],
            itemStyle: { color: '#4169e1' }
          },
          {
            name: '深圳证券交易所',
            value: [114.05, 22.54],
            itemStyle: { color: '#32cd32' }
          },
          {
            name: '台湾证券交易所',
            value: [121.56, 25.03],
            itemStyle: { color: '#ffd700' }
          },
          {
            name: '香港交易所',
            value: [114.15, 22.28],
            itemStyle: { color: '#ff8c00' }
          },
        ]
      }
    ]
  };

  myChart.setOption(option);
};

const handleResize = () => {
  if (myChart) {
    myChart.resize();
  }
};
</script>

<template>
  <div class="map-container">
    <div ref="MapRef" style="width: 100%; height: 100%;"></div>
    <!-- 添加股票成交数量展示盒子 -->
    <div class="stock-volume-box">
      <h3>股票成交数量</h3>
      <div class="stock-item">
        <span class="stock-name">上证指数</span>
        <span class="stock-value">{{ stockData.shanghai }}</span>
      </div>
      <div class="stock-item">
        <span class="stock-name">深证成指</span>
        <span class="stock-value">{{ stockData.shenzhen }}</span>
      </div>
      <div class="stock-item">
        <span class="stock-name">创业板指</span>
        <span class="stock-value">{{ stockData.chuangye }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.map-container {
  width: 100%;
  height: 100%;
  position: relative; /* 设置相对定位，以便子元素使用绝对定位 */
}

.stock-volume-box {
  position: absolute;
  bottom: 10px;
  left: 10px;
  background-color:#fffbf0;
  color: var(--theme-four);
  padding: 10px 15px;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 10; /* 确保盒子显示在地图上方 */
}

.stock-volume-box h3 {
  margin: 0 0 10px 0;
  font-size: 14px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
  padding-bottom: 5px;
}

.stock-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
  font-size: 12px;
}

.stock-name {
  color: #ccc;
}

.stock-value {
  font-weight: bold;
  color: var(--theme-two);
}
</style>