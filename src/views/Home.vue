<script setup>
import Head from '@/components/Head.vue';
import Main from '@/components/Main.vue';
import Footer from '@/components/Footer.vue';

</script>

<template>
    <dv-full-screen-container>
        <div class="home-box">
            <!-- 标题 -->
            <Head/>
            <!-- 中间 -->
             <Main/>
            <!-- 底部 -->
            <Footer/>
        </div>
    </dv-full-screen-container>
</template>

<style scoped>
.home-box{
    margin:calc(var(--theme-margin) - 15px);
}
</style>