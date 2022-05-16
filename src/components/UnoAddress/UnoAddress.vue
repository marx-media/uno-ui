<template>
  <address>
    <span v-if="name" class="block font-bold" v-text="name" />
    <span v-if="streetHouseNo" class="block" v-text="streetHouseNo" />
    <span v-if="postalCodeCity" class="block" v-text="postalCodeCity" />
    <div v-if="phone || mail" class="mt-5">
      <div v-if="phone" class="flex items-center">
        <div class="i-ph-phone" />
        <a :href="`tel:${phone}}`" class="ml-2" v-text="phone" />
      </div>
      <div v-if="mail" class="flex items-center">
        <div class="i-ph-at" />
        <a :href="`mailto:${mail}}`" class="ml-2" v-text="mail" />
      </div>
    </div>
  </address>
</template>

<script lang="ts" setup>
/* eslint-disable vue/prop-name-casing */
import { computed, defineComponent } from 'vue';
const props = defineProps({
  name: String,
  street: String,
  house_no: String,
  postal_code: String,
  city: String,
  phone: String,
  mail: String,
});
defineComponent({
  name: 'UnoAddress',
});

const streetHouseNo = computed(() =>
  `${props.street} ${props.house_no}`.trim(),
);
const postalCodeCity = computed(() => {
  const values = [];
  if (props.postal_code) values.push(props.postal_code);
  if (props.city) values.push(props.city);
  return values.join(', ');
});
</script>

<style></style>
