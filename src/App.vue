<template>
  <div id="app" class="px-8 py-8 min-h-screen">
    <span v-if="isLoading">loading...</span>
    <base-editor
      v-bind="{ items }"
      @submit="onSubmit"
      @remove="onRemove"
      @cancel="onCancel"
      :loading="isLoading"
      ref="editor"
    >
      <template slot="show" slot-scope="{ item }">
        <custom-show class="p-4" v-bind="{ item }"></custom-show>
      </template>

      <template slot="form" slot-scope="{ item }">
        <custom-editor-form class="p-4" v-model="item"></custom-editor-form>
      </template>
    </base-editor>
  </div>
</template>

<script>
import BaseEditor from "./components/BaseEditor/BaseEditor";
import CustomShow from "./components/CustomShow";
import CustomEditorForm from "./components/CustomEditorForm";
// TODO:
// empty states

const exps = [
  { id: 1, text: "A testing da test", whatever: [] },
  { id: 2, text: "B testing da test", whatever: ["c"] },
  { id: 3, text: "C testing da test", whatever: [] },
  { id: 4, text: "D testing da test", whatever: ["a", "b", "c"] }
];

export default {
  name: "App",
  components: {
    BaseEditor,
    CustomShow,
    CustomEditorForm
  },
  data() {
    return {
      items: exps,
      isLoading: false
    };
  },
  methods: {
    onSubmit(item) {
      this.isLoading = true;
      console.log("sending request...", item);
      setTimeout(() => {
        this.isLoading = false;
        console.log("request send");
        this.$refs.editor.closeAll();
      }, 2000);
    },
    onRemove(item) {
      this.items = this.items.filter(a => a.id !== item.id);
    },
    onCancel() {
      this.$refs.editor.closeAll();
    }
  }
};
</script>

<style>
.transform-move {
  transition: transform 1s;
}
</style>
