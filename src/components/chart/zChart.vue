<script setup>
import * as echarts from 'echarts';
import { ref, onMounted, onUnmounted } from 'vue';

// 图表容器引用
const zChartRef = ref(null);
// 图表实例
let myChart = null;

/**
 * 格式化时间工具
 * 使用echarts内置的时间格式化功能
 */
var formatTime = echarts.time.format;

/**
 * 生成模拟数据
 * 包含交易时间段数据和午休断点
 */
var _data = generateData1();

/**
 * 图表配置项
 */
const option = {
  // 使用UTC时间来处理时区问题
  useUTC: true,
  title: {
    text: '股市成交趋势切换单只股票涨跌情况',
    left: 'center',
    textStyle: {
          fontSize: 16,
          fontWeight: 'bold',
          color: '#8a6244'
    }
  },
  tooltip: {
    show: true,
    trigger: 'axis' // 坐标轴触发提示框
  },
  xAxis: [
    {
      type: 'time', // 时间轴
      interval: 1000 * 60 * 30, // 30分钟间隔
      axisLabel: {
        showMinLabel: true, // 显示最小刻度标签
        showMaxLabel: true, // 显示最大刻度标签
        formatter: (value, index, extra) => {
          if (!extra || !extra.break) {
            // 格式化正常时间点为 HH:mm 格式
            return formatTime(value, '{HH}:{mm}', true);
          }
          // 仅在断点开始处显示标签
          if (extra.break.type === 'start') {
            return (
              formatTime(extra.break.start, '{HH}:{mm}', true) +
              '/' +
              formatTime(extra.break.end, '{HH}:{mm}', true)
            );
          }
          return '';
        }
      },
      breakLabelLayout: {
        // 禁用断点标签自动移动以避免重叠
        moveOverlap: false
      },
      breaks: [
        {
          start: _data.breakStart, // 断点开始时间(午休开始)
          end: _data.breakEnd, // 断点结束时间(午休结束)
          gap: 0 // 断点间隔
        }
      ],
      breakArea: {
        expandOnClick: false, // 点击不展开断点区域
        zigzagAmplitude: 0, // 断点区域锯齿振幅
        zigzagZ: 200 // 断点区域Z轴高度
      }
    }
  ],
  yAxis: {
    type: 'value',
    min: 'dataMin', // 最小值从数据中自动计算
    axisLabel: {
          fontSize: 15
    }
  },
  dataZoom: [
    {
      type: 'inside', // 内置缩放组件
      xAxisIndex: 0
    },
    {
      type: 'slider', // 滑动条缩放组件
      xAxisIndex: 0,
      top: '90%' // 位于底部90%位置
    }
  ],
  series: [
    {
      type: 'line', // 折线图
      symbolSize: 0, // 不显示数据点
      data: _data.seriesData // 数据源
    }
  ]
};

/**
 * 生成随机模拟数据
 * @returns {Object} 包含交易数据和断点信息的对象
 */
function generateData1() {
  var seriesData = [];
  var time = new Date('2024-04-09T09:30:00Z'); // 开盘时间
  var endTime = new Date('2024-04-09T15:00:00Z').getTime(); // 收盘时间
  var breakStart = new Date('2024-04-09T11:30:00Z').getTime(); // 午休开始时间
  var breakEnd = new Date('2024-04-09T13:00:00Z').getTime(); // 午休结束时间
  var val = 1669; // 初始值

  // 生成时间序列数据
  while (time.getTime() <= endTime) {
    // 跳过午休时间
    if (time.getTime() <= breakStart || time.getTime() >= breakEnd) {
      // 生成随机波动值
      val = val + Math.floor((Math.random() - 0.5 * Math.sin(val / 1000)) * 20 * 100) / 100;
      val = +val.toFixed(2); // 保留两位小数
      seriesData.push([time.getTime(), val]); // 添加时间戳和对应值
    }
    time.setMinutes(time.getMinutes() + 1); // 每次增加1分钟
  }

  return {
    seriesData: seriesData,
    breakStart: breakStart,
    breakEnd: breakEnd
  };
}

/**
 * 初始化图表
 */
function initChart() {
  if (zChartRef.value && !myChart) {
    // 创建图表实例
    myChart = echarts.init(zChartRef.value);
    // 设置图表配置
    myChart.setOption(option);
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

// 组件挂载时初始化图表并监听窗口大小变化
onMounted(() => {
  // 添加延迟确保DOM完全渲染
  setTimeout(() => {
    initChart();
  }, 100);
  window.addEventListener('resize', handleResize);
});

// 组件卸载时销毁图表实例和移除事件监听
onUnmounted(() => {
  if (myChart) {
    myChart.dispose();
    myChart = null;
  }
  window.removeEventListener('resize', handleResize);
});
</script>

<template>
  <div class="z-chart">
    <div ref="zChartRef" style="width: 100%; height: 100%; min-height: 250px;"></div>
  </div>
</template>

<style scoped>
.z-chart {
  width: 100%;
  height: 100%;
  padding: 10px;
  box-sizing: border-box;
}
</style>