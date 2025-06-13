<template>
    <div class="binary-toggle">
        <input type="checkbox" :id="inputId" class="checkbox-input" :checked="modelValue"
            @change="$emit('update:modelValue', ($event.target as HTMLInputElement).checked)" />
        <label :for="inputId">{{ props.modelValue ? props.trueLabel : props.falseLabel }}</label>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps({
    falseLabel: {
        type: String,
        default: "Off"
    },
    trueLabel: {
        type: String,
        default: "On"
    },
    modelValue: {
        type: Boolean,
        required: true
    }
});

defineEmits(['update:modelValue']);

// 生成唯一ID
const inputId = ref(`binary-toggle-${Math.random().toString(36).substring(2, 9)}`);
console.log(inputId.value);
</script>

<style scoped>
.ticket-switch {
    display: flex;
    align-items: center;
    margin: 15px 0;
    gap: 10px;
}

input[type="checkbox"].checkbox-input {
    opacity: 0;
    position: absolute;
}

input[type="checkbox"].checkbox-input+label {
    position: relative;
    user-select: none;
    cursor: pointer;
    padding-left: 50px;
    /* 为开关留出空间 */
    min-height: 24px;
    display: flex;
    align-items: center;
    font-size: 16px;
    color: #e2e8f0;
}

/* 开关轨道 - ::before */
input[type="checkbox"].checkbox-input+label::before {
    content: "";
    position: absolute;
    left: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 40px;
    height: 20px;
    background-color: #3e6396;
    border: 1px solid #475569;
    border-radius: 20px;
    transition: background-color 0.3s;
}

/* 开关按钮 - ::after */
input[type="checkbox"].checkbox-input+label::after {
    content: "";
    position: absolute;
    left: 2px;
    top: 50%;
    transform: translateY(-50%);
    width: 16px;
    height: 16px;
    background-color: #94a3b8;
    border-radius: 50%;
    transition: all 0.3s;
}

/* 选中状态 */
input[type="checkbox"].checkbox-input:checked+label::before {
    background-color: #2563eb;
}

input[type="checkbox"].checkbox-input:checked+label::after {
    background-color: #e2e8f0;
    left: 22px;
    /* 40px宽度 - 16px按钮 - 2px边距 */
}
</style>