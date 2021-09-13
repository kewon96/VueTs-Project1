<template>
  <div>
    <h1>123</h1>
    <TodoInput :item="todoText" @input="updateTodoText" @add="addTodoItem" />

<!--    <input type="text" v-model="todoText">-->
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TodoInput from "./components/TodoInput.vue";

export default defineComponent({
  name: 'App',
  components: {
    TodoInput
  },
  data() {
    return {
      todoText: ''
    }
  },
  methods: {
    updateTodoText(value: string) {
      if(typeof value === 'string') {
        this.todoText = value;
      }
    },
    addTodoItem() {
      const value = this.todoText;
      localStorage.setItem(value, value)
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = '';
    }
  }
})
</script>