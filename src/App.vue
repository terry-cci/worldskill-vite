<script lang="ts">
export class TodoItem {
  name: string;
  color: string;
  done: boolean;

  constructor(name: string, color: string) {
    this.name = name;
    this.color = color;
    this.done = false;
  }
}

export const DEFAULT_COLOR = "#0070e8";
</script>

<script setup lang="ts">
import { ref, watch, computed, reactive } from "vue";
import TodoListVue from "./components/TodoList.vue";
import NewTodoForm from "./components/NewTodoForm.vue";

const todoList = ref([new TodoItem("Finish homework", "#008000")]);

// saving
watch(
  todoList,
  (v) => {
    localStorage.setItem("todoListItems", JSON.stringify(todoList.value));
  },
  { deep: true }
);
const savedTodoList = localStorage.getItem("todoListItems");
if (savedTodoList) {
  todoList.value = JSON.parse(savedTodoList);
}

const doneTodo = computed(() => todoList.value.filter(({ done }) => done));

const addTodo = ({ name, color }: { name: string; color: string }) => {
  todoList.value.push(new TodoItem(name, color));
};
const deleteTodo = (idx: number) => {
  todoList.value.splice(idx, 1);
};
const switchDoneOfTodo = (idx: number) => {
  todoList.value[idx].done = !todoList.value[idx].done;
};

const editingTodoIdx = ref<number | null>(null);
const editTodo = (idx: number) => {
  editingTodoIdx.value = idx;
};
const editingTodo = computed(() =>
  editingTodoIdx.value !== null ? todoList.value[editingTodoIdx.value] : null
);

const saveNewTodo = (todo: TodoItem) => {
  if (editingTodoIdx.value !== null) {
    Object.assign(todoList.value[editingTodoIdx.value], todo);
    editingTodoIdx.value = null;
  } else {
    if (!todo.name.length) return;
    addTodo(todo);
  }
};
</script>

<template>
  <h1 class="todo-list__title">
    Vue Todo List ({{ doneTodo.length }} / {{ todoList.length }})
  </h1>

  <TodoListVue
    :todo-list="todoList"
    @edit="editTodo"
    @delete="deleteTodo"
    @switch-done="switchDoneOfTodo"
  />

  <NewTodoForm :editing-todo="editingTodo" @save="saveNewTodo" />
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #222;
  color: #ddd;

  display: grid;
  place-items: center;

  min-height: 100vh;
}

a {
  color: unset;
}

.todo-list__new-todo-form {
  display: flex;
  align-items: center;
}

.todo-list__new-todo-name {
  margin-left: 0.5em;
}

.todo-list__new-todo-name,
.todo-list__new-todo-color {
  height: 2em;
}

.todo-list__new-todo-btn {
  margin-left: 1em;
}
</style>
