<script setup>

</script>

<template>
  <section class="info-box">
    <header class="info-box__header">
      <slot name="header">Информация</slot>
    </header>
    <main class="info-box__body">
      <slot />
    </main>
    <footer v-if="$slots.footer" class="info-box__footer">
      <slot name="footer" />
    </footer>
  </section>
</template>

<style scoped>
  .info-box__header {
    background-color: #73bc79;
    color: #000;
  }

  .info-box__footer {
    background-color: #8c5858;
  }
</style>