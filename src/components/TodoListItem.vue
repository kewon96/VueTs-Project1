<template>
  <li>
    <div>
      <span class="item"
            :class="todoItemClass"
            @click="toggleItem">{{ todoItem.title }}</span>
      <button @click="removeItem">delete</button>
    </div>
  </li>
</template>

<script lang="ts">
import {defineComponent, PropType} from "vue";
import {Todo} from "../App.vue";

export default defineComponent({
  name: 'TodoListItem',
  props: {
    todoItem: Object as PropType<Todo>,
    index: Number
  },
  computed: {
    todoItemClass(): string | null {
      if(!this.todoItem) {
        return null;
      }

      return this.todoItem.done ? 'complete' : null
    }
  },
  methods: {
    toggleItem() {
      this.$emit('toggle', this.todoItem, this.index);
    },
    removeItem() {
      this.$emit('remove', this.index);
    }
  }

})
</script>

<style scoped>
  div {
    display: flex;
    justify-content: flex-start;
  }

  .item {
    margin-right: 5px;
    cursor: pointer;
  }

  .complete {
    text-decoration: line-through;
  }
</style>