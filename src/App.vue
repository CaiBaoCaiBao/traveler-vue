<script setup lang="ts">
import { RouterView } from 'vue-router'
import HeaderComponent from '@/components/HeaderComponent/HeaderComponent.vue';
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-header><HeaderComponent/></el-header>
      <el-main>
        <RouterView />
      </el-main>
    </el-container>
  </div>
  
</template>

<style scoped>

</style>
