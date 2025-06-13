<template>
    <div class="attraction-view">
        <SideBar />
        <main id="main-content">
            <BaseInfoSection class="section-group" />
            <TicketSection class="section-group" />
        </main>
    </div>
</template>

<script setup lang="ts">
import SideBar from '@/views/AttractionView/SideBar.vue';
import BaseInfoSection from '@/views/AttractionView/Sections/BaseInfoSection.vue';
import OrderSection from '@/views/AttractionView/Sections/OrderSection.vue';
import ReservationSection from '@/views/AttractionView/Sections/ReservationSection.vue';
import TicketSection from '@/views/AttractionView/Sections/TicketSection.vue';
</script>

<style scoped lang="scss">
@use 'style/index.scss';
</style>