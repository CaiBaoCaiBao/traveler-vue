<template>
    <!-- 桌面版侧边栏 -->
    <aside id="sidebar" :class="{ 'collapsed': isCollapsed, 'mobile-hidden': isMobile }">
        <ul>
            <li class="logo">
                <div>
                    <h1 v-if="!isCollapsed">PAFTL</h1>
                </div>
                <div class="expend_arrow" :class="{ expend: isExpand }" @click="handleExpand">
                    <ChevronRightIcon />
                </div>
            </li>
            <li v-for="item in AsideItem" :key="item.id" :class="{ active: ActiveItem === item.link }">
                <a :href="item.link" @click="handleActice(item.link)">
                    <component :is="item.icons" />
                    <span v-if="!isCollapsed">{{ item.name }}</span>
                </a>
            </li>
        </ul>
    </aside>

    <!-- 移动版底部导航栏 -->
    <nav id="mobile-nav" v-if="isMobile">
        <ul>
            <li v-for="item in AsideItem" :key="item.id" :class="{ active: ActiveItem === item.link }">
                <a :href="item.link" @click="handleActice(item.link)">
                    <component :is="item.icons" />
                    <span>{{ item.name }}</span>
                </a>
            </li>
        </ul>
    </nav>
</template>

<script setup lang="ts">
import DescriptionIncon from '@/assets/svg/DescriptionIcon.vue'
import PersonIcon from '@/assets/svg/PersonIcon.vue'
import ShoppingModeIcon from '@/assets/svg/ShoppingModeIcon.vue'
import ReservationIcon from '@/assets/svg/ReservationIcon.vue'
import GroupIcon from '@/assets/svg/GroupIcon.vue'
import ChevronRightIcon from '@/assets/svg/ChevronRightIcon.vue'
import ReceiptLongIcon from '@/assets/svg/ReceiptLongIcon.vue'
import { ref, markRaw, onMounted, onBeforeUnmount } from 'vue'

const AsideItem = ref([
    { id: 1, name: '基本信息', icons: markRaw(DescriptionIncon), link: '#base-info' },
    { id: 2, name: '票务管理', icons: markRaw(ShoppingModeIcon), link: '#ticket' },
    { id: 3, name: '预约管理', icons: markRaw(ReservationIcon), link: '#reservation' },
    { id: 4, name: '订单管理', icons: markRaw(ReceiptLongIcon), link: '#order-list' },
    { id: 5, name: '成员信息', icons: markRaw(GroupIcon), link: '#personnel' },
    { id: 6, name: '个人信息', icons: markRaw(PersonIcon), link: '#personal-info' },
])

const ActiveItem = ref('#base-info')
const isExpand = ref(false)
const isCollapsed = ref(false)
const isMobile = ref(false)

const handleActice = (link: string) => {
  ActiveItem.value = link
}

const handleExpand = () => {
  isExpand.value = !isExpand.value
  isCollapsed.value = !isCollapsed.value
}

// 修改后的响应式处理
const checkScreenSize = () => {
  const width = window.innerWidth
  isMobile.value = width <= 768

  // 当宽度小于992px时默认折叠
  if (width < 992) {
    isCollapsed.value = true
    isExpand.value = false
  } else {
    // 大于992px时保持展开
    isCollapsed.value = false
    isExpand.value = false
  }
}

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', checkScreenSize)
})
</script>

<style scope lang="scss">
@use 'style/sidebar-desktop.scss';
@use 'style/sidebar-mobile.scss';
@use 'style/media/sidebar.scss';

</style>