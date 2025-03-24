<script setup lang="ts">
defineProps<{
  status: string;
}>();
</script>

<template>
  <UAlert v-if="status" color="neutral" :title="status">
    {{ status }}
  </UAlert>
</template>
