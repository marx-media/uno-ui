<template>
  <component :is="tag" class="btn" v-bind="bindings"><slot /></component>
</template>

<script lang="ts" setup>
import { computed, defineComponent, useAttrs } from 'vue';
import { RouterLink } from 'vue-router';

defineComponent({
  name: 'UnoBtn',
});

const attrs = useAttrs();

const tag = computed(() => (attrs.to ? RouterLink : 'button'));
const bindings = computed(() => ({
  type: attrs.to ? undefined : attrs.type ?? 'button',
}));
</script>

<style>
.btn {
  @apply bg-transparent px-2 py-1 inline-flex items-center;
}
.btn > div[class^='i-'] + *,
.btn > div[class^='i-']:last-child {
  @apply ml-1;
}
</style>
