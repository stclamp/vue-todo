<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['addTodo'])

const newTodo = ref<string>('')

const addTodo = () => {
  emit('addTodo', newTodo)
  newTodo.value = ''
}
</script>

<template>
  <form class="todo-form" @submit.prevent="addTodo()" @keyup.enter="addTodo()">
    <input
      class="todo-input"
      v-model="newTodo"
      name="newTodo"
      placeholder="New ToDo"
      autocomplete="off"
    />
    <button class="todo-button">Add ToDo</button>
  </form>
</template>

<style lang="scss" scoped>
@import '@/assets/utils/_variables.scss';
.todo-form {
  display: flex;
  min-width: 50%;
  margin-top: 20px;
}
.todo-input {
  border-radius: 3px;
  background: $color-dark-gray;
  width: 80%;
  min-height: 35px;
  border: 1px solid $color-gray;
  padding: 0 15px;

  &::placeholder {
    color: $color-black;
  }
}

.todo-button {
  padding: 0 15px;
  border: 1px solid $color-gray;
  border-radius: 3px;
  background: $color-dark-gray;
  margin-left: 15px;
  transition: 0.1s ease-in-out;

  &:hover {
    background: $color-dark-gray-hover;
  }

  &:active {
    transform: scale(0.95);
  }
}
</style>
