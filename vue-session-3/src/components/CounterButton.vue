<script setup>
import { ref } from 'vue'
const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  step: {
    type: Number,
    default: 1,
  }
})
const emit = defineEmits(['change'])
const count = ref( 0 )
const increment = () => {
  count.value = count.value + props.step
  emit('change', count.value)
}
</script>

<template>
  <button type="button" @click="increment">
    {{ label }}: {{ count }}
  </button>
</template>

<style scoped>

</style>