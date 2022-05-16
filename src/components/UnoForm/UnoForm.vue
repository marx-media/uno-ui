<template>
  <div class="uno-form-wrapper">
    <div
      class="items-center justify-center flex-col"
      :class="[thanks ? 'flex' : 'hidden']"
    >
      <slot name="thanks">
        <h2 class="font-bold text-2xl text-center">Thank You</h2>
        <h3 class="font-medium text-center">
          Your message has been delivered.
        </h3>
        <p class="mt-3">We will get back to you as soon as possible.</p>
        <uno-btn @click="setThanks(false)">
          <div class="i-ph-caret-left" />
          <div>Back</div>
        </uno-btn>
      </slot>
    </div>
    <form
      ref="formElement"
      class="uno-form"
      :class="{ hidden: thanks }"
      @submit.prevent="onFormSubmit"
    >
      <slot :payload="payload" />
      <uno-input
        v-if="captcha"
        v-model="payload.captcha"
        type="checkbox"
        name="captcha"
        label="Captcha"
        :class="{ hidden: !debug }"
      />
      <div class="flex">
        <slot name="submit">
          <uno-btn type="reset" class="btn-reset mr-2">{{
            cancelText
          }}</uno-btn>
          <uno-btn type="submit" class="btn-submit">{{ submitText }}</uno-btn>
        </slot>
      </div>
      <pre v-if="debug" v-text="payload" />
    </form>
  </div>
</template>

<script lang="ts" setup>
import { defineComponent, ref } from 'vue';
import { whenever } from '@vueuse/core';
import { UnoBtn, UnoInput } from '../index';
const props = defineProps({
  captcha: Boolean,
  submitText: {
    type: String,
    required: false,
    default: 'Submit',
  },
  cancelText: {
    type: String,
    required: false,
    default: 'Cancel',
  },
  reset: {
    type: Boolean,
    default: false,
  },
  debug: Boolean,
});
const emits = defineEmits(['formSubmit', 'update:reset']);
defineComponent({
  name: 'UnoForm',
});

const payload = ref<any>({});
const thanks = ref<boolean>(false);
const formElement = ref<HTMLFormElement>();

const setThanks = (b: boolean) => {
  thanks.value = b;
};
const onFormSubmit = () => {
  emits('formSubmit', payload.value);
  setThanks(true);
};
whenever(
  () => props.reset,
  () => {
    // Reset Form
    if (formElement.value) formElement.value.reset();
    // Reset Payload
    payload.value = {};
    // Reset "reset" :)
    emits('update:reset', false);
  },
);
</script>

<style></style>
