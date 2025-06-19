<template>
  <section class="login-view">
    <div class="form-head">
      <h1 class="form-title">{{ Role }}</h1>
      <div class="change-role">
        <el-text class="mx-1" type="primary" @click="changeRole">Change Role</el-text>
      </div>
    </div>
    <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules" label-width="auto">
      <el-form-item label="Username" prop="username" class="form-item">
        <el-input v-model="ruleForm.username" required />
      </el-form-item>
      <el-form-item label="Password" prop="password" class="form-item">
        <el-input v-model="ruleForm.password" type="password" show-password required />
      </el-form-item>
      <el-form-item prop="code">
        <div class="code-inputs">
          <el-input
            v-for="(_, index) in 4"
            :key="index"
            v-model="ruleForm.code[index]"
            @input="handleCodeInput(index, $event)"
            @keydown.delete="handleCodeDelete(index, $event)"
            maxlength="1"
            class="code-item"
          />
          <el-button
            type="primary"
            @click="handleCode"
            :disabled="countdown > 0"
            :class="{ 'code-btn-disabled': countdown > 0 }"
          >
            {{ countdown > 0 ? `${countdown}s后重新获取` : '获取验证码' }}
          </el-button>
        </div>
      </el-form-item>
      <el-form-item class="operate-btn">
        <el-button type="primary" @click="submitForm(ruleFormRef)"> Create </el-button>
        <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
      </el-form-item>
    </el-form>
    <div class="agreement">
      <el-checkbox v-model="isAgree" />
      <span>I agree <RouterLink class="agreement-link" to="/">PAFTL</RouterLink></span>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { RouterLink } from 'vue-router'

interface RuleForm {
  username: string
  password: string
  code: string[]
}
const ruleFormRef = ref<FormInstance>()
const ruleForm = ref<RuleForm>({
  username: '',
  password: '',
  code: [], // 初始化4个空字符串
})
const roleDialogVisible = ref(false)
const Role = ref('Attraction')
const isAgree = ref(false)
const countdown = ref(0)
const rules = ref<FormRules>({
  username: [{ required: true, message: 'Please input Username', trigger: 'blur' }],
  password: [{ required: true, message: 'Please input Password', trigger: 'blur' }],
})

// 验证码输入处理
const handleCodeInput = (index: number, event: Event) => {
  // 自动聚焦到下一个输入框
}

// 验证码删除处理
const handleCodeDelete = (index: number, event: KeyboardEvent) => {
  // 如果当前为空且不是第一个输入框，则删除前一个并聚焦
}

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!', {
        ...ruleForm.value,
        code: ruleForm.value.code.join(''), // 将数组拼接为字符串
      })
    } else {
      console.log('error submit!', fields)
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
  // 手动重置code数组
  ruleForm.value.code = []
}
// 模拟发送验证码
const handleCode = () => {
  ruleForm.value.code = ['1', '2', '3', '4']
  // 重新发送倒计时
  countdown.value = 60
  const timer = setInterval(() => {
    countdown.value--
    if (countdown.value <= 0) {
      clearInterval(timer)
    }
  }, 1000)
}

// 打开角色选择卡
const changeRole = () => {
  roleDialogVisible.value = true
}
</script>

<style scoped lang="scss">
@use './style/index.scss';
</style>
