<script setup>
const props = defineProps({
  todo: {
    type: Object,
    required: true
  }
})
const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <li class="todo-item">
    <label class="todo-item__label">
      <input
          type="checkbox"
          class="todo-item__checkbox"
          :checked="todo.done"
          @change="emit('toggle', todo.id)"
      />
      <span
          class="todo-item__title"
      >
        {{ todo.title }}
      </span>
    </label>
    <button
      class="todo-item__button"
      @click="emit('delete', todo.id)"
    >
      Удалить
    </button>
  </li>
</template>

<style scoped>

.todo-item {
  display: flex;
  padding-left: 10px;
  align-items: center;
  gap: 10px;
  border: 1px solid #fff;
}

.todo-item__button {
  font-size: 1rem;
  width: fit-content;
  height: 40px;
  padding: 10px 15px;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
  cursor: pointer;
  transition: all .3s ease;

  &:hover {
    opacity: 0.8;
    scale: 1.04;
  }
}

.todo-item__label {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 10px;
}

.todo-item__checkbox {
  appearance: none;
  border: 1px solid #fff;
  width: 25px;
  height: 25px;
  border-radius: 5px;
  position: relative;

  &:checked::after {
    position: absolute;
    content: "";
    width: 12px;
    height: 12px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: url('./../assets/icon-checkbox.svg') center/contain no-repeat;
  }
}

.todo-item__title {
  width: 100%;
  font-size: 1rem;
}
</style>