<script setup lang="ts">
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'
import StudentCard from '@/components/StudentCard.vue'

interface Student {
  id: string
  name: string
  surname: string
  gpa: number
}

const students = ref<Student[]>([])
const error = ref('')

onMounted(() => {
  StudentService.getStudents()
    .then((response) => {
      students.value = response.data
    })
    .catch((err) => {
      error.value = 'Failed to fetch students.'
    })
})
</script>

<template>
  <h1>Student List</h1>
  <div v-if="error">{{ error }}</div>
  <div v-else class="students">
    <StudentCard v-for="student in students" :key="student.id" :student="student" />
  </div>
</template>

<style scoped>
.students {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  justify-content: center;
}
.student-card {
  border: 1px solid #39495c;
  border-radius: 8px;
  padding: 16px;
  width: 220px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.07);
  background: #fff;
  text-align: center;
}
</style>
