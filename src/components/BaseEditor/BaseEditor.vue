<template>
  <div class="base-editor--layout w-full">
    <editor-heading
      :disabled="isAnyElementEditing"
      @click="addNew"
    ></editor-heading>

    <editor-list :items="innerItems">
      <template slot-scope="{ item, i }">
        <div class="show--container" v-if="!isElementEditing(item)" :key="i">
          <div
            class="show--toolbar flex w-full bg-grey-light p-2 align-center justify-end"
          >
            <button
              @click="onEdit(item);"
              class="bg-white hover:bg-grey-lightest text-grey-darkest py-2 px-4 rounded"
            >
              edit
            </button>
            <button
              @click="onRemove(item);"
              class="bg-white hover:bg-grey-lightest text-grey-darkest py-2 px-4 ml-2 rounded"
            >
              remove
            </button>
          </div>
          <slot name="show" v-bind="{ item }"></slot>
        </div>

        <div class="editor--container w-full" v-if="isElementEditing(item)">
          <validation-observer tag="form" ref="obs">
            <div slot-scope="{ invalid, validated }">
              <slot name="form" v-bind="{ item: items[i] }"></slot>

              <slot name="controls">
                <footer class="toolbar bg-grey-lighter p-2 flex">
                  <button
                    type="button"
                    class="bg-blue hover:bg-blue-dark text-white py-2 px-4 mr-2 border rounded"
                    :class="{ 'opacity-50': loading || (validated && invalid) }"
                    :disabled="loading || (validated && invalid)"
                    @click="onSaveChanges(item);"
                  >
                    Save changes
                  </button>
                  <button
                    type="button"
                    class="bg-transparent hover:bg-red text-red-dark font-semibold hover:text-white py-2 px-4 border border-red hover:border-transparent rounded"
                    @click="onCancel(item);"
                  >
                    cancel
                  </button>
                </footer>
              </slot>
            </div>
          </validation-observer>
        </div>
      </template>
    </editor-list>
  </div>
</template>

<script>
import EditorHeading from "./BaseEditorHeading";
import EditorList from "./BaseEditorList";
import { ValidationObserver } from "vee-validate";

class Model {
  constructor() {
    this.id = null;
    this.text = "";
    this.whatever = [];
    this.isDirty = true;
  }
}

export default {
  name: "BaseEditor",
  props: ["items", "loading"],
  components: {
    EditorHeading,
    EditorList,
    ValidationObserver
  },
  data() {
    return {
      editing: [],
      innerItems: [],
      oldVal: null
    };
  },
  watch: {
    items: {
      handler(val) {
        this.innerItems = val;
      },
      immediate: true
    }
  },
  computed: {
    isAnyElementEditing() {
      return this.editing.length > 0;
    }
  },
  methods: {
    addNew() {
      const newModel = new Model();
      newModel.id = Math.random()
        .toString(36)
        .substr(2, 9);
      this.editing.unshift(newModel.id);
      this.innerItems.unshift(newModel);
    },
    onEdit(item) {
      if (this.isAnyElementEditing || this.loading) return;
      this.closeAll();
      this.oldVal = { ...item };
      this.editing.push(item.id);
    },
    onRemove(item) {
      if (this.isAnyElementEditing || this.loading) return;
      this.$emit("remove", item);
    },
    validateForm() {
      return this.$refs.obs.validate();
    },
    async onSaveChanges(item) {
      if (item.isDirty) {
        delete item.isDirty;
      }
      const itemIndex = this.getItemIndex(item);
      this.innerItems.splice(itemIndex, 1, item);
      this.$emit("submit", item);
    },
    onCancel(item) {
      this.closeAll();
      const itemIndex = this.getItemIndex(item);
      if (item.isDirty) {
        this.innerItems.splice(itemIndex, 1);
        return;
      }
      this.innerItems.splice(itemIndex, 1, this.oldVal);
      this.$emit("cancel", item);
    },
    closeAll(item) {
      this.editing = [];
    },
    isElementEditing(item) {
      return this.getEditingIndex(item) > -1;
    },
    getEditingIndex(item) {
      return this.editing.findIndex(idx => idx === item.id);
    },
    getItemIndex(item) {
      return this.innerItems.findIndex(a => a.id === item.id);
    }
  }
};
</script>
