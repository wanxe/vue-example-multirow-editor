<template>
  <div class="custom-editor-form w-full">
    <p class="text-sm text-grey-dark">Your text here</p>
    <validation-provider name="example" rules="required">
      <template slot-scope="{ errors, valid }">
        <input
          class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white"
          :class="{ 'border-red': errors.length }"
          type="text"
          name="test"
          v-model="computedValue.text"
        />
        <span
          v-if="errors.length"
          v-text="errors[0]"
          class="text-red text-xs"
        ></span>
      </template>
    </validation-provider>

    <p class="text-sm text-grey-dark pt-4">Your checkboxes</p>
    <validation-provider name="checkbox" rules="required">
      <template slot-scope="{ errors, valid }">
        <ul class="list-reset mt-2">
          <li v-for="(key, index) in whateverList" :key="index">
            <label class="block text-grey-darker text-sm font-bold mb-2">
              {{ key }}
              <input
                type="checkbox"
                v-model="computedValue.whatever"
                :class="{ 'border-red': errors }"
                :value="key"
              />
            </label>
          </li>
        </ul>
        <span
          v-if="errors.length"
          v-text="errors[0]"
          class="text-red text-xs"
        ></span>
      </template>
    </validation-provider>
  </div>
</template>

<script>
import { ValidationProvider } from "vee-validate";

export default {
  props: ["value"],
  components: {
    ValidationProvider
  },
  data() {
    return {
      whateverList: ["a", "b", "c"]
    };
  },
  computed: {
    computedValue: {
      get() {
        return this.value;
      },
      set(val, oldVal) {
        this.$emit("input", val, oldVal);
      }
    }
  }
};
</script>
