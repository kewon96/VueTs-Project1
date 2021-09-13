<template>
  <div>
    <label for="todo-input">오늘 할 일 : </label>
    <input id="todo-input"
           type="text"
           :value="item"
           @input="handleInput"
           @keypress.enter="addTodo" />
    <button @click="addTodo">add</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: 'TodoInput',
  // props: ['item'],
  props: {
    item: {
      type: String,
      required: true
    }
  },
  methods: {
    handleInput(event: InputEvent) {
      // input tag에 있는 input Event하고 $emit에 있는 input과 다른 의미
      // input tag에 있는 input은 키보드로 입력하는 것을 의미하는 input
      // $emit에 있는 input은 컴포넌트간에 대화하기 위한 수단
      const eventTarget = event.target as HTMLInputElement;
      this.$emit('input', eventTarget.value);
    },
    addTodo() {
      this.$emit('add')
    },
  }
})
</script>