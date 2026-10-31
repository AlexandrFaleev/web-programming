<script setup>
import { ref, computed } from "vue";

/////////////////// TASK 1 ///////////////////
const mark = ref( 0 )
/////////////////// TASK 2 ///////////////////
const shoppingList = ref( [{id: 'product-1', name: 'Капуста'}, {id: 'product-2', name: 'Перец'}] )
const productName = ref( '' )
const productsCount = computed(() => shoppingList.value.length)
const addProduct = () => {
  if(productName.value.length > 0){
    shoppingList.value.push({
      id: crypto?.randomUUID() ?? Date.now().toString(),
      name: productName.value,
    })
    productName.value = ''
  }
}
const removeProduct = (productId) => {
  shoppingList.value = shoppingList.value.filter(({id}) => id !== productId)
}
/////////////////// TASK 3 ///////////////////
const categories = [
    {id: 'programming', name: 'Программирование'},
    {id: 'design', name: 'Дизайн'},
    {id: 'data', name: 'Данные'}
]
const courses = [
  {id: 'course-1', name: 'JavaScript', category: 'programming'},
  {id: 'course-2', name: 'UX/UI', category: 'design'},
  {id: 'course-3', name: 'SQL Impala', category: 'data'},
  {id: 'course-4', name: 'TypeScript', category: 'programming'},
  {id: 'course-5', name: 'Figma', category: 'design'},
  {id: 'course-6', name: 'Microsoft SQL Server', category: 'data'}
]
const searchQuery = ref('')
const selectedCategory = ref('all')
const filteredCourses = computed(() => {
  return courses.filter((course) => {
    return selectedCategory.value === 'all'
      ? course.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      : course.name.toLowerCase().includes(searchQuery.value.toLowerCase()) &&
          course.category === selectedCategory.value
  })
})
/////////////////// TASK 4 ///////////////////
const students = ref( [
  {id: 'student-1', name: 'Фалеев Александр', score: 90},
  {id: 'student-2', name: 'Иванов Иван', score: 75},
  {id: 'student-3', name: 'Гарри Поттер', score: 85},
  {id: 'student-4', name: 'Гермиона Грейнджер', score: 80},
  {id: 'student-5', name: 'Рон Уизли', score: 70},
  {id: 'student-6', name: 'Драко Малфой', score: 45}
] )
const stats = computed(() => {
  return {
    maxScore: students.value.length > 0 ? Math.max(...students.value.map(student => student.score)) : '-',
    minScore: students.value.length > 0 ? Math.min(...students.value.map(student => student.score)) : '-',
    averageScore: students.value.length > 0 ? Number(
        (students.value.reduce((sum, student) => sum + student.score, 0) /
            students.value.length).toFixed(1)
    ) : '-',
    passedCount: students.value.filter(student => student.score >= 60).length
  }
})
const removeStudent = (id) => {
  students.value = students.value.filter(student => student.id !== id)
}
const increaseStudentScore = (id) => {
  students.value = students.value.map((student) => {
    return student.id === id
        ? {...student, score: student.score + 5}
          : student
  })
}
const decreaseStudentScore = (id) => {
  students.value = students.value.map((student) => {
    return student.id === id
        ? {...student, score: student.score - 5}
        : student
  })
}
</script>

<template>
  <header class="header">
    <h1 class="header__header">Домашнее задание 2. Фалеев Александр</h1>
  </header>
  <section class="task-1">
    <label class="task-1__label">
      <span class="task-1__field-title">Оценка:</span>
      <input
          type="number"
          step="5"
          v-model.number="mark"
          class="task-1__field"
      />
    </label>
    <h2 v-if="mark >= 90 && mark <= 100" class="task-2__mark">
      Отлично
    </h2>
    <h2 v-else-if="mark >= 75 && mark < 90" class="task-2__mark">
      Хорошо
    </h2>
    <h2 v-else-if="mark >= 60 && mark < 75" class="task-2__mark">
      Удовлетворительно
    </h2>
    <h2 v-else-if="mark >= 0 && mark < 60" class="task-2__mark">
      Не сдал
    </h2>
    <h2 v-else class="task-2__mark">
      Ошибка: число от 0 до 100
    </h2>
  </section>
  <section class="task-2">
    <form class="task-2__form" @submit.prevent="addProduct">
      <label class="task-2__label">
        <span class="task-2__field-title">Название продукта</span>
        <input
            type="text"
            v-model.trim="productName"
            class="task-2__field"
        />
      </label>
      <button class="task-2__button" type="submit">
        Добавить
      </button>
    </form>
    <p class="task-2__counter">
      Количество товаров: {{productsCount}}
    </p>
    <ul class="task-2__list" v-if="productsCount > 0">
      <li class="task-2__product" v-for="product in shoppingList" :key="product.id">
        <span class="task-2__product-name">
          {{product.name}}
        </span>
        <button
            class="task-2__product-button"
            type="button"
            @click="removeProduct(product.id)"
        >
          Удалить
        </button>
      </li>
    </ul>
    <h2 class="task-2__empty" v-else>
      Список пуст
    </h2>
  </section>
  <section class="task-3">
    <div class="task-3__filters">
      <input
          type="search"
          placeholder="Поиск"
          v-model.trim="searchQuery"
          class="task-3__search"
      />
      <label class="task-3__category">
        <span>Категория:</span>
        <select v-model="selectedCategory" class="task-3__select">
          <option value="all">Все</option>
          <option
              v-for="category in categories"
              :value="category.id"
              :key="category.id"
          >
            {{category.name}}
          </option>
        </select>
      </label>
    </div>
    <ul class="task-3__list" v-if="filteredCourses.length > 0">
      <li
          v-for="course in filteredCourses"
          class="task-3__item"
          :key="course.id"
      >
        {{ course.name }}
      </li>
    </ul>
    <h2 v-else>
      Курсы не найдены
    </h2>
  </section>
  <section class="task-4">
    <h2 class="task-4__stats">
      <span>Максимум: {{ stats.maxScore  }}</span>
      <span>Минимум: {{ stats.minScore  }}</span>
      <span>Средняя: {{stats.averageScore  }}</span>
      <span>Кол-во сдавших: {{ stats.passedCount }}</span>
    </h2>
    <ul class="task-4__list" v-if="students.length > 0">
      <li class="task-4__item" v-for="student in students" :key="student.id">
        <p class="task-4__student-name">
          {{ students.indexOf(student) + 1 }}. {{ student.name }}
        </p>
        <p class="task-4__mark">
          {{ student.score }}
        </p>
        <div class="task-4__item-buttons">
          <button
              type="button"
              class="task-4__item-button"
              @click="decreaseStudentScore(student.id)"
          >
            -5
          </button>
          <button
              type="button"
              class="task-4__item-button"
              @click="increaseStudentScore(student.id)"
          >
            +5
          </button>
          <button
              type="button"
              class="task-4__item-button"
              @click="removeStudent(student.id)"
          >
            Удалить
          </button>
        </div>
      </li>
    </ul>
    <h2 v-else>
      Список студентов пуст
    </h2>
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
  flex-direction: row;
  align-items: center;
  gap: 50px;
  position: relative;
  padding: 50px 100px;

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

.task-1__label {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  align-items: center;
}

.task-1__field-title {
  font-size: 1.5rem;
  color: #fff;
}

.task-1__field {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  text-align: center;
  font-size: 1.2rem;
  background-color: transparent;
  border: 1px solid #fff;
  color: #fff;
  padding: 10px;
}

/*====== TASK 2 ======*/

.task-2 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  gap: 15px;
  padding: 50px 100px;

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

.task-2__form {
  display: flex;
  align-items: end;
  gap: 20px;
}

.task-2__label {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  max-width: 500px;
}

.task-2__field-title {
  font-size: 1.5rem;
  color: #fff;
}

.task-2__field {
  background-color: transparent;
  border: 1px solid #fff;
  padding: 10px 16px;
  font-size: 1.2rem;
  color: #fff;
  height: 40px;
}

.task-2__button,
.task-2__product-button{
  grid-area: button;
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

.task-2__counter {
  font-size: 1.2rem;
  color: #fff;
}

.task-2__list {
  align-self: center;
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 0;
  width: 500px;
}

.task-2__product {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  list-style-type: none;
  border-bottom: 1px solid #fff;
}

.task-2__product-name {
  font-size: 1.2rem;
  color: #fff;
}

.task-2__empty {
  align-self: center;
  font-size: 2rem;
}

/*====== TASK 3 ======*/

.task-3 {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  gap: 20px;
  min-height: 350px;

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

.task-3__filters {
  display: flex;
  align-items: end;
  gap: 40px;
}

.task-3__search {
  width: 350px;
  padding: 10px 16px;
  background-color: transparent;
  border: none;
  border-bottom: 1px rgba(255, 255, 255, 0.5) solid;
  color: rgba(255, 255, 255, 0.5);
  font-size: 1.2rem;

  &:focus {
    outline: none;
    border-color: #fff;
    color: #fff;
  }

  &:focus::placeholder {
    color: #fff;
  }

  &::placeholder {
    font-size: 1.2rem;
  }
}

.task-3__category {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.task-3__select {
  background-color: transparent;
  border: 1px solid #fff;
  outline: none;
  color: #fff;
  font-size: 1rem;
  padding: 8px;
  border-radius: 5px;
}

.task-3__list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  padding: 0;
}

.task-3__item {
  list-style-type: none;
  padding: 10px;
  border: 1px #fff dashed;
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

.task-4__stats {
  display: flex;
  align-items: center;
  gap: 20px;
  color: #fff;

  span:not(:last-child) {
    border-right: 1px solid #fff;
    padding-right: 1rem;
  }
}

.task-4__list {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding: 0;
  width: 50%;
  align-self: center;
}

.task-4__item {
  display: flex;
  align-items: center;
  column-gap: 10px;
  width: 100%;
  border: 1px solid #fff;
  padding-left: 16px;
}

.task-4__student-name {
  font-size: 1rem;
  color: #fff;
  width: 100%;
}

.task-4__mark {
  font-size: 1.5rem;
  color: #fff;
  margin-right: 10px;
}

.task-4__item-buttons {
  display: flex;
  align-items: center;
  gap: 10px;
}

.task-4__item-button {
  grid-area: button;
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

</style>
