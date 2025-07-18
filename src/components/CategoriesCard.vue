<script setup lang="ts">
import type { Event } from '@/types'

defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="categories-card">
    <div>{{ event.category }}</div>
    <div>{{ event.organizer }}</div>
  </div>
</template>

<style scoped>
.categories-card {
  text-align: right;
  font-size: 16px;

  padding: 20px;
  width: 200px;
  cursor: pointer;
  border: 1px solid #39495c;
  margin-bottom: 18px;
}

.categories-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
}
</style>
