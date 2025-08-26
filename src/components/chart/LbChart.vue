<script setup>
import { color } from 'echarts';
import { ref, onMounted } from 'vue';

const industryData = [
  { name: '银行', value: 7.5 },
  { name: '非银金融', value: 6.2 },
  { name: '食品饮料', value: 8.3 },
  { name: '家用电器', value: 5.1 },
  { name: '农林牧渔', value: 3.8 },
  { name: '医药生物', value: 7.9 },
  { name: '汽车', value: 4.6 },
  { name: '电力设备', value: 6.7 },
  { name: '电子', value: 7.2 },
  { name: '计算机', value: 5.8 },
  { name: '通信', value: 3.6 },
  { name: '传媒', value: 3.4 },
  { name: '煤炭', value: 2.5 },
  { name: '石油石化', value: 3.1},
  { name: '有色金属', value: 4.2},
  { name: '钢铁', value: 2.3},
  { name: '基础化工', value: 5.3 },
  { name: '建筑材料', value: 2.7 },
  { name: '建筑装饰', value: 2.9 },
  { name: '轻工制造', value: 2.6 },
  { name: '机械设备', value: 4.5 },
  { name: '国防军工', value: 2.1 },
  { name: '公用事业', value: 3.3 },
  { name: '交通运输', value: 3.7 },
  { name: '房地产', value: 4.8 },
  { name: '商贸零售', value: 2.4 },
  { name: '社会服务', value: 2.8 },
  { name: '美容护理', value: 1.9 },
  { name: '纺织服饰', value: 2.2},
  { name: '综合', value: 1.6},
  { name: '环保', value: 2.0 }
];

// 轮播表配置
const config = ref({
  header: ['行业名称', '占比'],
  data: [],
  align: ['center', 'center'],
//   columnWidth: ['40%', '60%'],
  rowNum: 6,
  autoPlay: true,
  autoPlayDelay: 10,
  headerBGC:'#8a6244',
  oddRowBGC:'#ffeb87',
  evenRowBGC:'#f8d586',
  hoverPause: true
});

// 组件挂载时初始化数据
onMounted(() => {
  // 格式化数据，添加百分比符号和颜色标签
  config.value.data = industryData.map(item => [
    item.name,
    `${item.value}%`,
    // `<span style="display:inline-block;width:12px;height:12px;border-radius:50%;background-color:${item.color};margin-right:5px;"></span>${item.name}`
  ]);
});
</script>

<template>
  <div class="Lb-chart">
    <dv-scroll-board :config="config" style="width:100%;height:100%;min-height:230px;" />
  </div>
</template>

<style scoped>
.Lb-chart {
  width: 100%;
  height: 100%;
  padding: 5px;
  box-sizing: border-box;
}
</style>