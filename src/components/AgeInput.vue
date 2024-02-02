<template>
  <div class="flex items-center gap-4">
    <StudentImage :imageUrl="student?.image" :isInputFocused="isInputFocused" />
    <div>
      <label
        for="hours-old"
        class="font-['Koulen'] text block mb-2"
        :class="{ 'text-[#3D06D7]': isInputFocused }"
        >{{ student?.name }} is</label
      >
      <div class="flex items-center gap-2">
        <input
          id="hours-old"
          v-model="formattedAge"
          @input="handleInput"
          @focus="handleFocus"
          @blur="handleBlur"
          class="px-[8px] py-2 border-2 border-gray-300 focus-visible:outline-none focus:border-[#906FEE] rounded-md min-w-[72px]"
          :style="{ width: newWidth }"
          :placeholder="placeholder ?? '0'"
        />
        <div class="text">
          {{
            formattedAge === '' || formattedAge === '0' || formattedAge === '1'
              ? 'hour'
              : 'hours'
          }}
          old
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import StudentImage from './StudentImage.vue'
import { ref, defineProps, defineEmits, onMounted } from 'vue'
import type { Student } from '@/types'

const props = defineProps({
  student: Object as () => Student,
  placeholder: String as () => string | null,
})

const emits = defineEmits(['update:age'])

const isInputFocused = ref(false)
const newWidth = ref('')

const replaceThousandsSeparator = (value: string): string => {
  return value.replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}

const formattedAge = ref(
  replaceThousandsSeparator(props.student?.age?.toString() ?? '')
)

const updateWidth = () => {
  const inputWidth = formattedAge.value.length * 9 + 2 * 10
  newWidth.value = `${inputWidth}px`
}

const handleInput = () => {
  const inputValue = formattedAge.value.replace(/\D/g, '')
  const formattedValue = replaceThousandsSeparator(inputValue)

  formattedAge.value = formattedValue
  emits('update:age', formattedValue === '' ? null : parseInt(inputValue, 10))
  updateWidth()
}

const handleFocus = () => {
  isInputFocused.value = true
}

const handleBlur = () => {
  isInputFocused.value = false
}

onMounted(() => {
  updateWidth()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&family=Koulen&display=swap');
</style>
