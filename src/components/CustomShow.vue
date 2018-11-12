<template>
  <div class="custom-show">
    <div class="text-sm text-grey-dark">The text</div>
    <p v-text="item.text"></p>
    <ul class="list-reset text-sm text-grey">
      <li v-for="item in item.whatever" :key="item.id">Selected: {{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["item"]
};
</script>
