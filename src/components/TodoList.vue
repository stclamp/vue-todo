<script setup lang="ts">
import { onMounted, ref } from 'vue'
import type { Todo } from '../types'
import TodoItem from './TodoItem.vue'

interface TodoListProps {
  saveData: () => void
}

const { saveData } = defineProps<TodoListProps>()

const emit = defineEmits(['getTodos'])

const todos = ref<Todo[]>([])

if (localStorage.getItem('todos')) {
  todos.value = JSON.parse(localStorage.getItem('todos') as string)
}

onMounted(() => {
  emit('getTodos', todos)
})

const doneTodo = (index: number) => {
  todos.value[index].done = !todos.value[index].done
  saveData()
}

const removeTodo = (index: number) => {
  const confirm = window.confirm('Are you sure?')
  confirm && todos.value.splice(index, 1)
  saveData()
}

const saveEditedTodo = ({ index, newContent }: { newContent: string; index: number }) => {
  todos.value[index].content = newContent
  saveData()
}
</script>

<template>
  <ul class="todo-list">
    <TodoItem
      v-for="(todo, index) in todos"
      :key="index"
      :todo="todo"
      :doneTodo="doneTodo"
      :removeTodo="removeTodo"
      :index="index"
      @doneTodo="doneTodo"
      @saveEditedTodo="saveEditedTodo"
    />
  </ul>
</template>

<style lang="scss">
.todo-list {
  width: 40%;
  margin-top: 25px;
}

@media (width < 540px) {
  .todo-list {
    width: 90%;
  }
}
</style>
