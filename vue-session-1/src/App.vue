<script setup>
import {ref} from "vue";

/////////////////// TASK 1 ///////////////////
const title = ref('Александр')
const changeTitle = () => {
  title.value = 'Заголовок изменён'
}
/////////////////// TASK 2 ///////////////////
const counter = ref(0)
const step = ref(1)
const MIN = 0
const MAX = 20
const decrement = () => {
  counter.value = counter.value - step.value >= MIN
      ? counter.value - step.value : MIN
}
const increment = () => {
  counter.value = counter.value + step.value <= MAX
      ? counter.value + step.value : MAX
}
/////////////////// TASK 3 ///////////////////
const userName = ref('')
const minSymbols = 4
const maxSymbols = 12
/////////////////// TASK 4 ///////////////////
const theme = ref('dark')
const buttonText = ref('Включить светлый режим')
const toggleTheme = () => {
  theme.value = theme.value === 'dark' ? 'light' : 'dark'
  buttonText.value = theme.value === 'dark' ? 'Включить светлый режим' : 'Включить тёмный режим'
}
</script>

<template>
  <header class="header">
    <h1 class="header__header">Домашнее задание 1. Фалеев Александр</h1>
  </header>
  <section class="task-1">
    <h2 class="task-1__title">
      {{title}}
    </h2>
    <p class="task-1__paragraph">
      МИСИС
      <br>
      Веб-программирование
    </p>
    <button class="task-1__button" @click="changeTitle">
      Изменить заголовок
    </button>
  </section>
  <section class="task-2">
    <h2 class="task-2__title">
      {{counter}}
    </h2>
    <div class="task-2__controllers">
      <button
        class="task-2__button"
        :disabled="counter <= MIN"
        @click="decrement"
      >
        Уменьшить
      </button>
      <label class="task-2__label">
        <span>Шаг</span>
        <input
            class="task-2__input"
            v-model="step"
            type="number"
            min="1"
            step="1"
        />
      </label>
      <button
          class="task-2__button"
          :disabled="counter >= MAX"
          @click="increment"
      >
        Увеличить
      </button>
    </div>
  </section>
  <section class="task-3">
    <label class="task-3__label">
      <span>Введите имя:</span>
      <input
          v-model="userName"
          type="text"
          class="task-3__input"
      />
      <span class="task-3__counters">
        <span :style="userName.length >= maxSymbols && 'color:red !important;'">{{ userName.length }}</span>
         /
        {{ maxSymbols }}
      </span>
    </label>
    <h2 class="task-3__title">
      Привет, {{ userName }}!
    </h2>
    <button
        class="task-3__button"
        :disabled="userName.length < minSymbols"
    >
      Регистрация
    </button>
  </section>
  <section :class="`task-4 ${theme}-theme`">
    <button class="task-4__button" @click="toggleTheme">
      {{ buttonText }}
    </button>
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
  display: grid;
  align-items: center;
  height: fit-content;
  grid-template-areas:
    "header header"
    "paragraph button";
  gap: 15px;
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

.task-1__title,
.task-1__paragraph {
  color: var(--vt-c-white);
}

.task-1__title {
  font-size: 2rem;
  grid-area: header;
}

.task-1__paragraph {
  grid-area: paragraph;
}

.task-1__button {
  grid-area: button;
  font-size: 1rem;
  width: fit-content;
  padding: 10px 15px;
  margin-left: auto;
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

/*====== TASK 2 ======*/

.task-2 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  column-gap: 15px;

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

.task-2__title {
  font-size: 4rem;
  text-shadow: 0 0 5px #00f,
              0 0 10px #00f,
              0 0 15px #00f;
}

.task-2__controllers {
  display: flex;
  align-items: end;
  gap: 10px;
}

.task-2__button {
  grid-area: button;
  font-size: 1rem;
  width: fit-content;
  padding: 10px 15px;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
  cursor: pointer;
  transition: all .3s ease;
  height: 40px;

  &:not(:disabled):hover {
    opacity: 0.8;
    scale: 1.04;
  }

  &:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }
}

.task-2__label {
  display: flex;
  flex-direction: column;
  gap: 5px;
  align-items: center;

  span {
    font-size: 1.2rem;
    color: #fff;
  }
}

.task-2__input {
  width: fit-content;
  max-width: 100px;
  height: 40px;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
  font-size: 1.2rem;
  padding-inline: 15px;
  text-align: center;
}

/*====== TASK 3 ======*/

.task-3 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;

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

.task-3__label {
  display: flex;
  flex-direction: column;
  gap: 5px;
  align-items: start;
  width: 350px;
  align-self: start;
}

.task-3__input {
  font-size: 1rem;
  padding: 8px 10px;
  width: 100%;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
}

.task-3__counters {
  display: block;
  width: fit-content;
  align-self: end;
}

.task-3__title {
  font-size: 2rem;
  text-align: center;
  text-shadow: 0 0 5px #00f,
              0 0 10px #00f,
              0 0 15px #00f;
}

.task-3__button {
  grid-area: button;
  font-size: 1rem;
  width: fit-content;
  padding: 10px 15px;
  background-color: transparent;
  border: 1px #fff solid;
  color: #fff;
  cursor: pointer;
  transition: all .3s ease;
  height: 40px;
  align-self: center;

  &:not(:disabled):hover {
    opacity: 0.8;
    scale: 1.04;
  }

  &:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }
}

/*====== TASK 4 ======*/

.task-4 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
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

  &.dark-theme {
    background-color: transparent;
    color: #fff;
  }

  &.light-theme {
    background-color: #fff;
    color: #000;
    border-color: #000;

    &::after {
      border-color: #000;
      color: #000;
    }

    & * {
      border-color: #000 !important;
      color: #000 !important;
    }
  }
}

.task-4__button {
  grid-area: button;
  font-size: 1rem;
  width: fit-content;
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

</style>
