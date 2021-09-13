<template>
  <li>{{ todoItem }}</li>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: 'TodoListItem',
  props: {
    todoItem: String
  }

})
</script>