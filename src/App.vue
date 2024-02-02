<template>
  <div class="max-w-lg mx-auto py-20">
    <AgeInput
      :student="student"
      @update:age="updateStudentAge"
      :placeholder="placeholder"
    />
  </div>
</template>

<script setup lang="ts">
import AgeInput from './components/AgeInput.vue'
import { reactive, ref, watch } from 'vue'
import type { Student } from '@/types'

const savedStudentAge = localStorage.getItem('studentAge')
const initialStudent: Student = {
  name: 'Samuel',
  age: savedStudentAge ? JSON.parse(savedStudentAge) : null,
  image: 'student-image.jpg',
}

const student = reactive<Student>(initialStudent)

const placeholder = ref<string | null>('7')

const updateStudentAge = (newAge: number | null) => {
  student.age = newAge
}

watch(
  () => student.age,
  () => {
    localStorage.setItem('studentAge', JSON.stringify(student.age))
  }
)
</script>

<style scoped>
* {
  font-family: Inter;
}
</style>
