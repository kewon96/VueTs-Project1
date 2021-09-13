<template>
  <li>
    <span>{{ todoItem }}</span>
    <button @click="removeItem">delete</button>
  </li>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: 'TodoListItem',
  props: {
    todoItem: String,
    index: Number
  },
  methods: {
    removeItem() {
      this.$emit('remove', this.index);
    }
  }

})
</script>