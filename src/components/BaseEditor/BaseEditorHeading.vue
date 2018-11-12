<template>
  <div class="base-editor--heading flex justify-end py-4">
    <slot name="title"></slot>
    <button
      :class="[
        'bg-grey-light text-grey-darkest font-bold py-2 px-4 rounded inline-flex items-center',
        computedClass
      ]"
      :disabled="disabled"
      @click="$emit('click');"
    >
      <slot> + Add </slot>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    computedClass() {
      return this.disabled ? "opacity-50" : "hover:bg-grey";
    }
  }
};
</script>
