<script setup lang="ts">
interface Student {
  id: string
  name: string
  surname: string
  gpa: number
}

defineProps<{ student: Student }>()
</script>

<template>
  <div class="student-card">
    <h2>{{ student.name }} {{ student.surname }}</h2>
    <p>GPA: {{ student.gpa }}</p>
  </div>
</template>

<style scoped>
.student-card {
  border: 1px solid #39495c;
  border-radius: 8px;
  padding: 16px;
  width: 220px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.07);
  background: #fff;
  text-align: center;
  margin: 8px;
}
.student-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
}
</style>
