<script setup>
import capsuleChart from './chart/capsuleChart.vue';
// import kChart from './kChart.vue';
import zChart from './chart/zChart.vue';

</script>

<template>
    <div class="footer-box">
        <div style="flex: 0 1 44%;">
            <dv-border-box-8 :color="['#ffeb87','#f8d586']">
                <capsule-chart/>
            </dv-border-box-8>
        </div>

        <div style="flex: 0 1 55%;">
            <dv-border-box-6 :color="['#ffeb87','#f8d586']">
                <!-- <k-chart/> -->
                 <z-chart/>
            </dv-border-box-6>
        </div>
    </div>
</template>

<style scoped>
.footer-box{
    display: flex;
    align-items: center;
    justify-content: space-around;
    margin:calc(var(--theme-margin) - 10px);
    height: 35vh;
}
</style>