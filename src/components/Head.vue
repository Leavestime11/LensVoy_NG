<script setup lang="ts">
import { color } from 'echarts';

</script>

<template>
    <div class="head-box">
        <div style="flex: 0 1 20%;">
            <dv-decoration-3 :color="['#ffeb87','#f8d586']" style="height:5vh;" />
        </div>

        <div>
            <dv-decoration-7 :color="['#ffeb87','#ffeb87']"><span>
                LensVoy NG
            </span></dv-decoration-7>
        </div>

        <div style="flex: 0 1 70%;display: flex;">
            <dv-decoration-6 style="height:50px;" :color="['#ffeb87','#ffeb87']"/>
            <dv-decoration-6 style="height:50px;" :color="['#ffeb87','#ffeb87']"/>
            <dv-decoration-6 style="height:50px;" :color="['#ffeb87','#ffeb87']"/>
        </div>
    </div>
</template>

<style scoped>
.head-box{
    display: flex;
    align-items: center;
    color: rgb(77, 54, 5);
}
span{
    padding: 10px;
    font-weight: bold;
}
</style>