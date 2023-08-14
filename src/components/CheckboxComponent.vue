<script setup lang="ts">
interface CheckboxComponentProps {
  index: number
  checked: boolean
}

const emit = defineEmits(['checkboxChange'])

const { index } = defineProps<CheckboxComponentProps>()

const handleCheckboxChange = () => {
  emit('checkboxChange', index)
}
</script>

<template>
  <div class="check-group">
    <label :for="`checkbox-${index}`" class="checkbox">
      <input
        class="checkbox__input"
        type="checkbox"
        :id="`checkbox-${index}`"
        :checked="checked"
        @change="handleCheckboxChange"
      />
      <svg class="checkbox__icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 22">
        <rect width="21" height="21" x=".5" y=".5" fill="#FFF" stroke="#006F94" rx="3" />
        <path
          class="tick"
          stroke="#6EA340"
          fill="none"
          stroke-linecap="round"
          stroke-width="4"
          d="M4 10l5 5 9-9"
        />
      </svg>
    </label>
  </div>
</template>

<style lang="scss" scoped>
.checkbox {
  counter-increment: total;
}

input[type='checkbox']:checked {
  counter-increment: checked;
}

.checkbox {
  $block: &;

  cursor: pointer;
  display: flex;
  align-items: center;

  &__input {
    position: absolute;
    width: 1.375em;
    height: 1.375em;
    opacity: 0;
    cursor: pointer;

    &:checked + #{$block}__icon .tick {
      stroke-dashoffset: 0;
    }
  }

  &__icon {
    width: 1.375em;
    height: 1.375em;
    flex-shrink: 0;
    overflow: visible;

    .tick {
      stroke-dasharray: 20px;
      stroke-dashoffset: 20px;
      transition: stroke-dashoffset 0.2s ease-out;
    }
  }

  &__label {
    margin-left: 0.5em;
  }
}
</style>
