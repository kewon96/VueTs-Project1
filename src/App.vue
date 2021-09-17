<template>
  <div>
    <header>
      <h1>Hello Vue.Ts</h1>
    </header>
    <main>
      <TodoInput :item="content.todoText"
                 @inputtext="updateTodoText"
                 @add="addTodoItem" />
      <div>
        <ul>
          <TodoListItem v-for="(todoItem, index) in content.todoItems"
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

<script setup lang="ts">
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";
import {reactive} from "vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  /**
   * storage에 데이터 저장
   * 데이터를 어떻게 넣을 것인지 규격을 지정하는 중간단계역할
   * @param todoItem
   */
  save(todoItem: Todo[]) {
    // 직렬화(배열 -> 문자열)
    const parsed = JSON.stringify(todoItem);
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch(): Todo[] {
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

const content = reactive({
  todoText: '',
  todoItems: [] as Todo[]
})

const updateTodoText = (value: string) => {
  if(typeof value === 'string') {
    content.todoText = value;
  }
}

const addTodoItem = () => {
  const value = content.todoText;
  const todo: Todo = {
    title: value,
    done: false
  }

  // 기존의 items에 값을 밀어넣는다.
  content.todoItems.push(todo);
  // storage에 데이터 저장
  storage.save(content.todoItems);
  initTodoText();
}

const initTodoText = () => {
  content.todoText = '';
}

const fetchTodoItems = () => {
  content.todoItems = storage.fetch().sort((a, b) => {
    if( a.title > b.title ) {
      return 1;
    } else if( a.title < b.title ) {
      return -1;
    } else {
      return 0;
    }
  });
}

const toggleTodoItemComplete = (todoItem: Todo, index: number) => {
      content.todoItems.splice(index, 1, {
    ...todoItem,
    done: !todoItem.done,
  })
}

const removeTodoItem = (index: number) => {
  content.todoItems.splice(index, 1);
  storage.save(content.todoItems);
}

fetchTodoItems();

</script>
