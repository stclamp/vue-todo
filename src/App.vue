<script setup lang="ts">
import { ref } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'
import { type Todo } from './types'

const newTodo = ref<string>('')

const todos = ref<Todo[]>([])

const getTodos = ({ value }: { value: Todo[] }) => {
  todos.value = value
}

const addTodo = ({ value }: { value: string }) => {
  newTodo.value = value
  if (!newTodo.value) {
    alert('Please, fill the input')
  } else {
    todos.value.unshift({
      done: false,
      content: newTodo.value
    })
    newTodo.value = ''
    saveData()
  }
}

function saveData() {
  const storageData = JSON.stringify(todos.value)
  localStorage.setItem('todos', storageData)
}
</script>

<template>
  <div class="container">
    <div class="todo-app-wrapper">
      <h1>Todo App</h1>
      <TodoForm @addTodo="addTodo" />
      <TodoList :saveData="saveData" @getTodos="getTodos" />
      <h4 class="empty" v-if="!todos.length">List of todos is empty</h4>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '@/assets/utils/_variables.scss';
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 15px;
}

.empty {
  margin-top: 25px;
}

.todo-app-wrapper {
  padding: 20px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 30px;
  background: $color-gradient;
  box-shadow: $box-shadow;
  backdrop-filter: blur(7.5px);
}
</style>
