<template>
  <div id="app1">
    <h1>Список дел</h1>
    <input v-model="itemForAdd" class="mainInput" placeholder="Добавьте задачу" />
    <button @click="add">Добавить задачу</button>
    <button @click="del">Удалить выполненные задачи</button>

    <div class="filter">
      <label>
        <input type="radio" name="check" @click="filterTasks('completed')" />
        Выполненные
      </label>
      <label>
        <input type="radio" name="check" @click="filterTasks('notCompleted')" />
        Невыполненные
      </label>
      <label>
        <input type="radio" name="check" @click="filterTasks('all')" />
        Все задачи
      </label>
    </div>

    <div>
      <label for="sort">Сортировать по имени </label>
      <input type="radio" name="sort" @click="sortTasks('title')" />
      <p>Сортировать по дате:</p>
      <label>
        Самые ранние
        <input type="radio" name="sort" @click="sortTasks('asc')" />
      </label>
      <label>
        Самые поздние
        <input type="radio" name="sort" @click="sortTasks('desc')" />
      </label>
    </div>

    <h1>Мои задачи:</h1>
    <div v-for="(todo, index) in filteredTasks" :key="todo.id" style="display: flex; align-items: center;">
      <todo-item
        :todo="todo"
        :index="index"
        @complete-done="makeDone(index)"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  components: {
    TodoItem,
  },
  data() {
    return {
      todoList: [],
      itemForAdd: '',
      filter: 'all',
      sortBy: 'id',
    };
  },
  computed: {
    filteredTasks() {
      // Фильтрация задач
      let tasks = this.todoList.slice();
      if (this.filter === 'completed') {
        tasks = tasks.filter(todo => todo.completed);
      } else if (this.filter === 'notCompleted') {
        tasks = tasks.filter(todo => !todo.completed);
      }

      // Сортировка задач
      if (this.sortBy === 'title') {
        return tasks.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortBy === 'asc') {
        return tasks.sort((a, b) => a.id - b.id);
      } else if (this.sortBy === 'desc') {
        return tasks.sort((a, b) => b.id - a.id);
      }
      return tasks;
    },
  },
  methods: {
    // Метод для добавления задач
    add() {
      if (this.itemForAdd.trim() === '') return;
      const newTodo = {
        title: this.itemForAdd,
        completed: false,
        id: Date.now(),  // Уникальный ID на основе времени
      };
      this.todoList.push(newTodo);
      this.itemForAdd = '';
    },
    // Метод для удаления выполненных задач
    del() {
      this.todoList = this.todoList.filter(todo => !todo.completed);
    },
    // Метод для изменения статуса выполнения задачи
    makeDone(index) {
      this.todoList[index].completed = !this.todoList[index].completed;
    },
    // Метод для фильтрации задач
    filterTasks(filter) {
      this.filter = filter;
    },
    // Метод для сортировки задач
    sortTasks(sortBy) {
      this.sortBy = sortBy;
    },
  },
};
</script>

<style scoped></style>