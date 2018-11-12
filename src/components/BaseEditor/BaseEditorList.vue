<template>
  <collapse-transition tag="ul" group class="items--list list-reset w-full">
    <li
      v-for="(item, i) in items"
      :key="item.id || i"
      class="list--item rounded overflow-hidden shadow-lg mb-6"
    >
      <slot v-bind="{ item, i }"></slot>
    </li>
  </collapse-transition>
</template>

<script>
import { CollapseTransition } from "vue2-transitions";

export default {
  props: ["items"],
  components: {
    CollapseTransition
  }
};
</script>
