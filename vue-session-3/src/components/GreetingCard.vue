<script setup>
defineProps({
  name: {
    type: String,
    required: true,
  },
  age: {
    type: Number,
    default: 18
  },
  role: {
    type: String,
    default: 'Студент'
  },
})
</script>

<template>
  <li>
    {{ name }} {{ age }} {{ role }}
  </li>
</template>

<style scoped>

</style>