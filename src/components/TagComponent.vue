<template>
  <div class="tag">
    <span>
      {{ props.tag }}
    </span>
  </div>
</template>

<script setup lang="ts">
interface Tag {
  tag: string
}

const props = defineProps<Tag>()
</script>

<style scoped>
.tag {
  padding: 5px 10px;
  background-color: rgba(35, 194, 133, 0.2);
  border: 1.5px solid rgba(88, 237, 180, 0.3);
  border-radius: 8px;
  color: #5eead4;
  font-size: 14px;
  font-weight: 500;
  user-select: none;
  text-align: center;
}
</style>
