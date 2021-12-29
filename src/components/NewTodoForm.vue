<script setup lang="ts">
import { reactive, watch } from "vue";
import { DEFAULT_COLOR, TodoItem } from "../App.vue";

const newTodoForm = reactive({
  name: "",
  color: DEFAULT_COLOR,
});

const props = defineProps<{
  editingTodo: TodoItem | null;
}>();

const emit = defineEmits<{
  (e: "save", todo: TodoItem): void;
}>();

const saveNewTodo = () => {
  emit("save", new TodoItem(newTodoForm.name, newTodoForm.color));
  newTodoForm.name = "";
  newTodoForm.color = DEFAULT_COLOR;
};

watch(
  () => props.editingTodo,
  (todo) => {
    if (todo !== null) {
      newTodoForm.name = todo.name;
      newTodoForm.color = todo.color;
    } else {
      newTodoForm.name = "";
      newTodoForm.color = DEFAULT_COLOR;
    }
  }
);
</script>

<template>
  <form @submit.prevent="saveNewTodo" class="todo-list__new-todo-form">
    <label for="new-todo-name"
      >{{ editingTodo !== null ? "Update Todo" : "New Todo Item" }}:
    </label>
    <input
      type="text"
      id="new-todo-name"
      v-model="newTodoForm.name"
      class="todo-list__new-todo-name"
    />
    <input
      type="color"
      v-model="newTodoForm.color"
      class="todo-list__new-todo-color"
    />

    <button class="todo-list__new-todo-btn">
      {{ editingTodo !== null ? "Update" : "Add" }}
    </button>
  </form>
</template>
