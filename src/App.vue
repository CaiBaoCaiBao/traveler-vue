<script setup lang="ts">
import { useToggle } from '@vueuse/core'
import { isDark } from '@/utils/dark'
import { RouterView } from 'vue-router'
import Dark from '@/assets/svg/Dark.vue'
import Light from '@/assets/svg/Light.vue'
import HeaderComponent from '@/components/HeaderComponent/HeaderComponent.vue'

// 暗黑模式控制
const toggleDark = useToggle(isDark)
</script>

<template>
  <div>
    <header>
      <HeaderComponent />
    </header>
    <main>
      <RouterView />
      <div class="theme">
        <button @click="toggleDark()" class="theme-btn">
          <Dark v-if="!isDark" />
          <Light v-else />
        </button>
      </div>
    </main>
  </div>
</template>

<style scoped lang="scss">
.container {
  margin: 0;
}

.theme {
  position: fixed;
  right: 3%;
  bottom: 2%;
}

.theme-btn {
  box-shadow: 0 0 10px #909090;
  border: 1px solid #909090;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease-in-out;

  &:hover {
    transform: scale(1.1);
  }
}

@media (max-width: 768px) {
  .theme {
    right: 2%;
    bottom: 2%;
  }
}
</style>
