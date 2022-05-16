<template>
  <div
    v-if="showNavigation"
    class="fixed inset-0 z-20 bg-black/60"
    @click="setNavigation(false)"
  />
  <div
    class="fixed inset-y-0 w-3/4 right-0 bg-white z-30 transition transform py-5 px-3 flex flex-col"
    :class="[showNavigation ? 'translate-x-0' : 'translate-x-full']"
  >
    <slot name="top-mobile-nav" />
    <uno-btn
      v-for="item in navItems"
      :key="item.to"
      :to="item.to"
      :class="navItemClasses"
    >
      {{ item.text }}
    </uno-btn>
  </div>
  <div
    class="uno-app-bar px-3"
    :class="[[fixed && 'fixed inset-x-0 top-0 z-10']]"
    v-bind="attrs"
  >
    <div class="container mx-auto flex" :class="innerClasses">
      <slot />
      <slot name="actions">
        <div class="ml-auto hidden lg:block">
          <uno-btn
            v-for="item in navItems"
            :key="item.to"
            :to="item.to"
            :class="navItemClasses"
          >
            {{ item.text }}
          </uno-btn>
        </div>
        <div class="ml-auto block lg:hidden">
          <uno-btn @click="setNavigation(true)">
            <div class="i-ph-list text-xl" />
          </uno-btn>
        </div>
      </slot>
    </div>
  </div>
</template>

<script lang="ts" setup>
import type { PropType } from 'vue';
import { defineComponent, ref, useAttrs } from 'vue';
import { UnoBtn } from '../UnoBtn';

interface NavItem {
  text: string;
  to: string;
}

defineProps({
  innerClasses: Array,
  fixed: Boolean,
  navItems: {
    type: Array as PropType<NavItem[]>,
    default: () => [],
  },
  navItemClasses: String,
});

defineComponent({
  name: 'UnoAppBar',
  inheritAttrs: false,
});

const attrs = useAttrs();

const showNavigation = ref<boolean>(false);
const setNavigation = (b: boolean) => {
  showNavigation.value = b;
};
</script>

<style></style>
