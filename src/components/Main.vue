<script setup>
import MapChart from '@/components/chart/mapChart.vue'
import xChart from './chart/xChart.vue';
import LbChart from './chart/LbChart.vue';
import pChart from './chart/pChart.vue';

</script>

<template>
  <div class="main-box">
    <div style="flex: 0 1 50%;">
      <dv-border-box-7 style="height:55vh;" :color="['rgb(255,234,185,0.5)','#f8d586']">
        <MapChart/>
      </dv-border-box-7>
    </div>

    <div style="flex: 0 1 47%; ">
      <div>
        <dv-border-box-6 style="height:25vh;" :color="['#ffeb87','#f8d586']">
          <xChart/>
        </dv-border-box-6>
      </div>

      <div style="height:30vh; display: flex;">
        <div style="flex: 0 1 40%; ">
          <dv-border-box-1 :color="['#ffeb87','#f8d586']">
            <pChart/>
          </dv-border-box-1>
        </div>
        <div style="flex: 0 1 60%; ">
          <dv-border-box-8 :reverse="true" :color="['#ffeb87','#f8d586']">
            <LbChart/>
          </dv-border-box-8>
        </div>
      </div>
    </div>
  </div>
</template> 

<style scoped>
.main-box {
  display: flex;
  align-items: center;
  margin: calc(var(--theme-margin) - 10px);
  justify-content: space-around;
}
</style>