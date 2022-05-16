<template>
  <div
    class="uno-form-group"
    :class="[{ checkbox: attrs.type === 'checkbox' }]"
  >
    <slot name="label">
      <label v-if="label" class="uno-form-label block" v-text="label" />
    </slot>
    <div class="uno-input-group">
      <slot name="prependInput" />
      <uno-input v-bind="attrs" @update:model-value="onUpdateModelValue" />
      <slot name="appendInput" />
    </div>
    <slot name="hint" />
  </div>
</template>

<script lang="ts" setup>
import { defineComponent, useAttrs } from 'vue';
import { UnoInput } from '../index';
defineProps({
  label: String,
  modelValue: {
    type: [String, Number, Boolean],
  },
});
const emits = defineEmits(['update:modelValue']);
defineComponent({
  name: 'UnoFormGroup',
  inheritAttrs: false,
});
const attrs = useAttrs();
const onUpdateModelValue = (passThrough: string | number | boolean) =>
  emits('update:modelValue', passThrough);
</script>

<style>
.uno-form-group.checkbox {
  @apply flex items-center justify-end flex-row-reverse;
}
.uno-form-group.checkbox label {
  @apply ml-2;
}
</style>
