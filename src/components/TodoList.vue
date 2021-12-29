<script setup lang="ts">
import { TodoItem } from "../App.vue";

const props = defineProps<{
  todoList: TodoItem[];
}>();
const emit = defineEmits<{
  (e: "edit", idx: number): void;
  (e: "delete", idx: number): void;
  (e: "switchDone", idx: number): void;
}>();
</script>

<template>
  <ul class="todo-list__list">
    <li
      v-for="(item, idx) in todoList"
      :style="{ backgroundColor: item.color }"
      class="todo-list__list-item"
    >
      <input
        type="checkbox"
        @click="emit('switchDone', idx)"
        :checked="item.done"
      />
      <span class="todo-list__item-name">
        {{ item.name }}
      </span>
      <button @click="emit('edit', idx)" class="todo-list__item-btn">
        Edit
      </button>
      <button @click="emit('delete', idx)" class="todo-list__item-btn">
        Delete
      </button>
    </li>
  </ul>
</template>

<style>
.todo-list__list {
  padding: 0;
}

.todo-list__list-item {
  display: flex;
  align-items: center;
  padding: 0.5em 0.5em;
  border-bottom: 1px solid white;
}

.todo-list__list-item input[type="checkbox"] {
  outline: 1px solid white;
  margin-right: 1em;
}

.todo-list__list-item:last-child {
  border-bottom: none;
}

.todo-list__item-name {
  flex-grow: 1;
}

.todo-list__item-btn {
  margin: 0 0.5em;
}
</style>
