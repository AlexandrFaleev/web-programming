<script setup>
import { ref, computed } from 'vue'
/////////////////// TASK 1 ///////////////////
import GreetingCard from '@/components/GreetingCard.vue'
const people = [
  {id: 1, name: 'Александр', age: 24, role: 'Программист'},
  {id: 2, name: 'Павел', age: 20, role: 'Безработный'},
  {id: 3, name: 'Софья' },
]
/////////////////// TASK 2 ///////////////////
import CounterButton from '@/components/CounterButton.vue'
const apples = ref(0)
const pears = ref(0)
const totalFruits = computed(() => apples.value + pears.value)
/////////////////// TASK 3 ///////////////////
import InfoBox from '@/components/InfoBox.vue'
/////////////////// TASK 4 ///////////////////
import { useLocalStorage } from '@/composables/useLocalStorage.js'
import TodoItem from '@/components/TodoItem.vue'
const todos = useLocalStorage('todos', [])
const newTaskName = ref('')
const unDoneCount = computed(() => todos.value.filter(({done}) => !done).length)
const doneCount = computed(() => todos.value.filter(({done}) => done).length)
const addTodo = () => {
  const title = newTaskName.value.trim()

  if(title.length !== 0) {
    todos.value.push({
      id: crypto?.randomUUID() ?? Date.now().toString(),
      title,
      done: false
    })
  }

  newTaskName.value = ''
}
const toggleTodo = (id) => {
  todos.value = todos.value.map((todo) => {
    return todo.id === id ? {...todo, done: !todo.done} : todo
  })
}
const removeTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}
const removeUndone = () => {
  todos.value = todos.value.filter(({done}) => done)
}
</script>

<template>
  <header class="header">
    <h1 class="header__header">Домашнее задание 3. Фалеев Александр</h1>
  </header>
  <section class="task-1">
    <ul class="task-1__list">
      <GreetingCard
          v-for="person in people"
          class="task-1__item"
          :key="person.id"
          :name="person.name"
          :age="person.age"
          :role="person.role"
      />
    </ul>
  </section>
  <section class="task-2">
    <div class="task-2__buttons">
      <CounterButton
          class="task-2__button"
          label="Яблоки"
          :step="5"
          @change="apples = $event"
      />
      <CounterButton
          class="task-2__button"
          label="Груши"
          :step="5"
          @change="pears = $event"
      />
    </div>
    <h2 class="task-2__total">Всего фруктов: {{ totalFruits }}</h2>
  </section>
  <section class="task-3">
    <InfoBox>
      Без header'а и footer'а
    </InfoBox>
    <InfoBox>
      <template #header>
        А вот и header
      </template>
      С header'ом, но без footer'а
    </InfoBox>
    <InfoBox>
      <template #header>
        А вот и header
      </template>
        C header'ом и footer'ом
      <template #footer>
        А вот и footer
      </template>
    </InfoBox>
  </section>
  <section class="task-4">
    <div class="task-4__todo">
      <form class="task-4__form" @submit.prevent="addTodo">
        <label class="task-4__label">
          <input
              class="task-4__input"
              type="text"
              placeholder=" "
              v-model="newTaskName"
          />
          <span class="task-4__input-title">
          Название задачи
        </span>
        </label>
        <button class="task-4__add-button" type="submit">
          Добавить
        </button>
      </form>
      <h3>
        Осталось {{ unDoneCount }}
      </h3>
      <button
          class="task-4__remove-undone-button"
          v-if="doneCount > 0"
          @click="removeUndone"
          :disabled="unDoneCount === 0"
      >
        Очистить невыполненные
      </button>
      <ul class="task-4__list" v-if="todos.length > 0">
        <TodoItem
            class="task-4__todo-item"
            v-for="todo in todos"
            :todo="todo"
            :key="todo.id"
            @toggle="toggleTodo"
            @delete="removeTodo"
        />
      </ul>
      <h3 class="task-4__todo-empty" v-else>
        У вас пока нет задач
      </h3>
    </div>
  </section>
</template>

<style scoped>

.header {
  border-bottom: 3px #fff solid;
  width: 100%;
  padding-bottom: 1rem;
}

.header__header {
  text-align: center;
}

section {
  width: 100%;
  min-height: 40px;
  display: block;
  border: 2px #fff dashed;
  padding: 1rem;
}

/*====== TASK 1 ======*/

.task-1 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;
  padding: 50px;

  position: relative;

  &::after {
    content: "Упражнение 1";
    position: absolute;
    top: 0;
    right: 0;
    border-left: inherit;
    border-bottom: inherit;
    padding: .5rem;
    color: #fff;
  }
}

.task-1__list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.task-1__item {
  display: flex;
  border: 1px dashed #fff;
  padding: 10px 15px;
}

/*====== TASK 2 ======*/

.task-2 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;
  padding: 50px;

  position: relative;

  &::after {
    content: "Упражнение 2";
    position: absolute;
    top: 0;
    right: 0;
    border-left: inherit;
    border-bottom: inherit;
    padding: .5rem;
    color: #fff;
  }
}

.task-2__buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
}

.task-2__button{
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

/*====== TASK 3 ======*/

.task-3 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;
  padding: 70px 50px;

  position: relative;

  &::after {
    content: "Упражнение 3";
    position: absolute;
    top: 0;
    right: 0;
    border-left: inherit;
    border-bottom: inherit;
    padding: .5rem;
    color: #fff;
  }
}

/*====== TASK 4 ======*/

.task-4 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;
  padding: 50px;

  position: relative;

  &::after {
    content: "Упражнение 4";
    position: absolute;
    top: 0;
    right: 0;
    border-left: inherit;
    border-bottom: inherit;
    padding: .5rem;
    color: #fff;
  }
}

.task-4__todo {
  display: flex;
  flex-direction: column;
  row-gap: 24px;
  padding: 30px;
  border: 1px dashed #fff;
  width: 404px;
  min-height: 329px;
}

.task-4__form {
  display: flex;
  align-items: center;
  gap: 15px;
}

.task-4__label {
  width: 100%;
  position: relative;
}

.task-4__input {
  width: 100%;
  height: 40px;
  border: 1px solid #939191;
  outline: none;
  background-color: transparent;
  color: #fff;
  padding-inline: 16px;

  &:focus {
    border-color: #fff;
  }

  &:not(:placeholder-shown) ~ .task-4__input-title {
    transform: translate(-2.5em, -3.2em);
    color: #fff;
    scale: 0.85;
  }
}

.task-4__input-title {
  width: 200px;
  position: absolute;
  top: 50%;
  left: 17px;
  transform: translateY(-50%);
  color: #939191;
  transition: all .3s ease;
}

.task-4__add-button {
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

.task-4__remove-undone-button {
  font-size: 1rem;
  width: 100%;
  height: 40px;
  padding: 10px 15px;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
  cursor: pointer;
  transition: all .3s ease;

  &:not(:disabled):hover {
    opacity: 0.8;
    scale: 1.04;
  }

  &:disabled {
    cursor: not-allowed;
    color: #818181;
    border-color: #818181;
  }
}

.task-4__list {
  width: 100%;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.task-4__todo-empty {
  text-align: center;
  font-size: 18px;
  color: #fff;
}

</style>
