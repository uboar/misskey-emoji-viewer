<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  params: {
    value: string
  }
}>()

const showImage = ref(false)
</script>

<template>
  <div v-if="!showImage">
    <button @click="() => { showImage = true }">{{ props.params.value }}</button>
  </div>
  <img v-else :src="props.params.value" height="50px" />
</template>