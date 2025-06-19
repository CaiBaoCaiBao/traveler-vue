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
      <button class="edit-btn" @click="handleOpenForm">Edit</button>
    </div>
  </el-card>
  <!-- Base Info Form -->
  <el-drawer v-model="drawer" :direction="direction">
    <template #header>
      <h4>BaseInfo</h4>
    </template>
    <template #default>
      <el-form :model="formData" label-width="120px" label-position="top">
        <el-form-item label="Name">
          <el-input v-model="formData.name" placeholder="Please input name" />
        </el-form-item>

        <el-form-item label="Tags">
          <div class="tag-input-container">
            <el-tag
              v-for="(tag, index) in formData.tags"
              :key="index"
              closable
              @close="removeTag(index)"
            >
              {{ tag }}
            </el-tag>
            <el-input
              v-model="inputTag"
              class="input-new-tag"
              placeholder="Add tag"
              style="width: 100%"
              @keyup.enter="addTag"
              @blur="addTag"
            />
          </div>
        </el-form-item>

        <el-form-item label="Description">
          <el-input
            v-model="formData.description"
            type="textarea"
            :rows="4"
            placeholder="Please input description"
          />
        </el-form-item>
      </el-form>
    </template>
    <template #footer>
      <div style="flex: auto">
        <el-button @click="cancelClick">cancel</el-button>
        <el-button type="primary" @click="confirmClick" :loading="confirmLoading">
          confirm
        </el-button>
      </div>
    </template>
  </el-drawer>
</template>

<script setup lang="ts">
import Tag from '@/components/TagComponent.vue'
import { reactive, ref } from 'vue'
import { ElMessage } from 'element-plus'
import type { DrawerProps } from 'element-plus'
interface BaseInfo {
  name: string
  tags: string[]
  description: string
}
const Data = reactive<BaseInfo>({
  name: '',
  description: '',
  tags: [],
})
const data = reactive<BaseInfo>({ ...Data })
const formData = reactive<BaseInfo>({
  name: data.name,
  description: data.description,
  tags: [...data.tags],
})

const inputTag = ref('') // 用于存储正在输入的标签
const drawer = ref(false)
const direction = ref<DrawerProps['direction']>('rtl')
const confirmLoading = ref(false)

const handleOpenForm = () => {
  drawer.value = true
}

const addTag = () => {
  if (inputTag.value && !formData.tags.includes(inputTag.value)) {
    formData.tags.push(inputTag.value)
    inputTag.value = ''
  }
}

const removeTag = (index: number) => {
  formData.tags.splice(index, 1)
}

function cancelClick() {
  drawer.value = false
  // 重置表单数据
  Object.assign(formData, Data, {
    name: data.name,
    description: data.description,
    tags: [...data.tags],
  })
  inputTag.value = ''
}

function confirmClick() {
  confirmLoading.value = true
  setTimeout(() => {
    // 更新数据
    Object.assign(data, {
      name: formData.name,
      description: formData.description,
      tags: [...formData.tags],
    })
    confirmLoading.value = false
    drawer.value = false
    ElMessage({
      message: 'Congrats, this is a success message.',
      type: 'success',
    })
  }, 1000)
}
</script>

<style scoped lang="scss">
@use '../style/baseinfo.scss';
</style>
