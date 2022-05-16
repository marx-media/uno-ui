<template>
  <component :is="inputComponent" class="uno-input" @input="onInput" />
</template>

<script lang="ts" setup>
import { computed } from '@vue/reactivity';
import { defineComponent, useAttrs } from 'vue';

defineProps({
  modelValue: [String, Number, Boolean],
});

const emits = defineEmits(['update:modelValue']);

defineComponent({
  name: 'UnoInput',
});

const attrs = useAttrs();
const inputComponent = computed(() =>
  attrs.type === 'textarea' ? 'textarea' : 'input',
);

const onInput = (e: Event) => {
  const target = e.target as HTMLInputElement;

  // AUTOGROW TEXTAREA
  if (target.tagName === 'TEXTAREA')
    target.style.height = `${target.scrollHeight}px`;

  let value: string | number | boolean;
  switch (attrs.type) {
    case 'checkbox': {
      value = target.checked;
      break;
    }
    default: {
      value = target.value;

      break;
    }
  }
  emits('update:modelValue', value);
};
</script>

<style>
.uno-input {
  @apply w-full bg-transparent p-0 focus:outline-none;
}
.uno-input[type='checkbox'] {
  @apply w-auto;
}
textarea.uno-input {
  @apply resize-none min-h-20;
}
</style>
