<script setup lang="ts">
import { ref, watch } from 'vue'
import { type Todo } from '../types/'
import CheckboxComponent from './CheckboxComponent.vue'
import EditIcon from './EditIcon.vue'
import CancelIcon from './CancelIcon.vue'
import AcceptIcon from './AcceptIcon.vue'
interface TodoItemProps {
  doneTodo: (index: number) => void
  todo: Todo
  removeTodo: (index: number) => void
  index: number
}
const { index, todo } = defineProps<TodoItemProps>()

const emit = defineEmits(['doneTodo', 'saveEditedTodo'])

const editing = ref<boolean>(false)
const editedContent = ref<string>(todo.content)
const editInput = ref<HTMLInputElement | null>(null)

const startEditing = () => {
  editing.value = true
}

const saveEdit = () => {
  editing.value = false
  if (editInput.value?.value === '') {
    alert('Please, fill the input')
    editedContent.value = todo.content
  } else {
    emit('saveEditedTodo', { newContent: editedContent.value, index })
  }
}

const cancelEdit = () => {
  editing.value = false
  editedContent.value = todo.content
}

watch(editInput, () => {
  if (editInput.value) {
    editInput.value.focus()
  }
})

const handleCheckboxChange = () => {
  emit('doneTodo', index)
}
</script>

<template>
  <li class="todo-item">
    <CheckboxComponent :index="index" :checked="todo.done" @checkboxChange="handleCheckboxChange" />
    <span v-if="!editing" @click="doneTodo(index)" :class="{ done: todo.done }" class="content">{{
      todo.content
    }}</span>
    <input
      v-else
      v-model="editedContent"
      ref="editInput"
      tabindex="0"
      class="edit-input"
      @keyup.enter="saveEdit"
    />
    <div class="buttons">
      <button v-if="!editing" @click="startEditing" class="button edit-btn"><EditIcon /></button>
      <div class="save-buttons-wrapper" v-else>
        <button @click="saveEdit" class="button save-btn"><AcceptIcon /></button>
        <button @click="cancelEdit" class="button cancel-btn"><CancelIcon /></button>
      </div>

      <button @click="removeTodo(index)" class="remove-btn">Remove</button>
    </div>
  </li>
</template>

<style lang="scss" scoped>
@import '@/assets/utils/_variables.scss';
@import '@/assets/utils/_mixins.scss';
.todo-item {
  @include flex-between;
  width: 100%;
  margin-top: 20px;
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}

.content {
  white-space: pre-wrap;
}

.buttons {
  display: flex;
  align-items: self-end;
}

.edit-input {
  border: none;
  background: transparent;
  text-align: center;
  font-family: 'Montserrat';
  font-size: $fs-16;
  margin-left: 31px;
}

.save-buttons-wrapper {
  display: flex;
}

.button {
  border: none;
  background: transparent;
  margin-right: 10px;
}

.remove-btn {
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  background: $color-red;
  color: #fff;
  transition: 0.2s ease-in-out;

  &:hover {
    background: $color-red-hover;
  }

  &:active {
    transform: scale(0.95);
  }
}

@media (width < 540px) {
  .content,
  .edit-input {
    max-width: 30%;
  }
}
</style>
