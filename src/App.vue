<template>
  <div>
    <header>
      <h1>Vue Todo with Typescript</h1>
    </header>
    <TodoInput :item="todoText" @input="updateTodoText" @add="addTodoItem"></TodoInput>
    <div>
      <ul>
        <TodoListItem v-for="(todoItem, index) in todoItems" :key="index" :index="index" :todoItem="todoItem"
          @toggle="toggleTodoItemComplete" @remove="removeTodoItem"></TodoListItem>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref } from 'vue'
  import TodoInput from './components/TodoInput.vue'
  import TodoListItem from './components/TodoListItem.vue'

  const STORAGE_KEY = 'vue-todo-ts';
  const storage = {
    save(todoItems: Todo[]) {
      const parsed = JSON.stringify(todoItems);
      localStorage.setItem(STORAGE_KEY, parsed)
    },
    fetch(): Todo[] {
      const todoItems = localStorage.getItem(STORAGE_KEY) || '[]';
      const result = JSON.parse(todoItems);
      return result;
    }
  };

  export interface Todo {
    title: string;
    done: boolean;
  }

  export default defineComponent({
    setup() {
      const todoText = ref('');
      const todoItems = ref([] as Todo[]);

      const updateTodoText = (event: InputEvent) => {
        const eventTarget = event.target as HTMLInputElement;
        todoText.value = eventTarget.value;
      };
      const addTodoItem = () => {
        const value = todoText.value;
        const todo: Todo = {
          title: value,
          done: false
        }
        todoItems.value.push(todo);
        storage.save(todoItems.value);
        initTodoText();
      };
      const initTodoText = () => {
        todoText.value = '';
      };
      const fetchTodoItems = () => {
        todoItems.value = storage.fetch().sort((a, b) => {
          if (a.title < b.title) {
            return -1;
          } else if (a.title > b.title) {
            return 1;
          } else {
            return 0;
          }
        });
      };
      const removeTodoItem = (index: number) => {
        todoItems.value.splice(index, 1);
        storage.save(todoItems.value);
      };
      const toggleTodoItemComplete = (todoItem: Todo, index: number) => {
        todoItems.value.splice(index, 1, {
          ...todoItem,
          done: !todoItem.done
        });
        storage.save(todoItems.value);
      };

      return {
        todoText,
        todoItems,
        updateTodoText,
        addTodoItem,
        initTodoText,
        fetchTodoItems,
        removeTodoItem,
        toggleTodoItemComplete
      }
    },
    created() {
      this.fetchTodoItems();
    },
    components: {
      TodoInput,
      TodoListItem
    },
  })
</script>

<style scoped>

</style>