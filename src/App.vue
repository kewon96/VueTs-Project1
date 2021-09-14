<template>
  <div>
    <header>
      <h1>Hello Vue.Ts</h1>
    </header>
    <main>
      <TodoInput :item="todoText"
                 @input="updateTodoText"
                 @add="addTodoItem" />
      <div>
        <ul>
          <TodoListItem v-for="(todoItem, index) in todoItems"
                        :key="index"
                        :index="index"
                        :todoItem="todoItem"
                        @toggle="toggleTodoItemComplete"
                        @remove="removeTodoItem"/>
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  /**
   * storage에 데이터 저장
   * 데이터를 어떻게 넣을 것인지 규격을 지정하는 중간단계역할
   * @param todoItem
   */
  save(todoItem: any[]) {
    // 직렬화(배열 -> 문자열)
    const parsed = JSON.stringify(todoItem);
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch() {
    // localstorage에 있는 데이터를 조회
    const todoItems = localStorage.getItem(STORAGE_KEY) || '[]';

    // 컴포넌트에서 쓸수있는 형태로 변환
    return JSON.parse(todoItems);
  }
}

// 객체를 위한 타입
export interface Todo {
  title: string;
  done: boolean;
}

export default defineComponent({
  name: 'App',
  components: {
    TodoInput,
    TodoListItem
  },
  data() {
    return {
      todoText: '',
      todoItems: [] as Todo[]
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
      const todo: Todo = {
        title: value,
        done: false
      }

      // 기존의 items에 값을 밀어넣는다.
      this.todoItems.push(todo);
      // storage에 데이터 저장
      storage.save(this.todoItems);
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = '';
    },
    fetchTodoItems() {
      this.todoItems = storage.fetch();
    },
    toggleTodoItemComplete(todoItem: Todo, index: number) {
      this.todoItems.splice(index, 1, {
        ...todoItem,
        done: !todoItem.done,
      })
    },
    removeTodoItem(index: number) {
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    }
  },
  created() {
    this.fetchTodoItems();
  }
})
</script>