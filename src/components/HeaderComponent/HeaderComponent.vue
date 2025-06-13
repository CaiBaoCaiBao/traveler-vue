<template>
  <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
    <div id="logo">
      Logo
    </div>
    <div class="menu-list">
        <el-menu-item v-for="(item,index) in enumList" :key="index" :index=item.id class="menu-item">
            <RouterLink :to="item.url" class="menu-link">{{ item.name }}</RouterLink>
        </el-menu-item>
    </div>
  </el-menu>
</template>

<script setup lang="ts">
import { RouterLink } from 'vue-router'
import { ref } from 'vue'

const enumList = ref([
  { id: "1", name: 'Home', url: '/' },
  { id: "2", name: 'About', url: '/about' },
  { id: "3", name: 'Login', url: '/login' },
])

const activeIndex = ref("1")
const handleSelect = (key: number, keyPath: string[]) => {
  console.log(key, keyPath)
}
</script>

<style scoped lang="scss">
@use "style/general.scss"
</style>