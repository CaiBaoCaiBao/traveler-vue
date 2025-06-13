<template>
    <el-card id="base-info">
        <template #header>
            <div class="card-header">
                <h1>{{ data.name }}</h1>
            </div>
        </template>
        <div class="tagList">
            <Tag v-for="tag in data.tags" :key="tag" :tag="tag" />
        </div>
        <p class="descriptionText">
            {{ data.description }}
        </p>
        <div class="btn-group">
            <button class="edit-btn">Edit</button>
        </div>
    </el-card>
</template>

<script setup lang="ts">
import Tag from '@/components/TagComponent.vue';
import { reactive } from 'vue';
interface BaseInfo {
    name: string,
    tags: string[],
    description: string,
}
const Data = reactive<BaseInfo>({
    name: 'asfas',
    description: 'fasfa',
    tags: ['fasf1', 'fasf2', 'fasf3']
});
const data = reactive<BaseInfo>({ ...Data });
</script>

<style scoped lang="scss">
@use '../style/baseinfo.scss';
</style>