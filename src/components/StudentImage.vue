<template>
  <div
    class="rounded-full w-[90px] h-[90px] bg-gray-300 bg-no-repeat bg-cover"
    :class="{
      'outline outline-offset-4 outline-1 outline-[#3D06D7]':
        props.isInputFocused,
    }"
    :style="{
      backgroundImage: props.imageUrl ? `url('${props.imageUrl}')` : 'none',
    }"
  ></div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps(['imageUrl', 'isInputFocused'])
</script>
