<script setup lang="ts">
import { Ref, ref } from "vue";

interface Todo {
  text: string;
  isCompleted: boolean;
}

const todos: Ref<Todo[]> = ref([
  {
    text: "rtewste",
    isCompleted: true,
  },
]);

const input: Ref<HTMLInputElement | null> = ref(null);

const toggleTodo = (index: number) => {
  todos.value[index].isCompleted = !todos.value[index].isCompleted;
};

const addTodo = (e: Event) => {
  if (input.value) {
    todos.value.unshift({
      text: input.value.value || "New Todo",
      isCompleted: false,
    });
    input.value.value = "";
  }
};

const deleteTodo = (index: number) => {
  todos.value = todos.value.filter((_, id) => id !== index);
};
</script>

<template>
  <div class="flex flex-col items-center py-10">
    <form @submit.prevent="addTodo" class="mb-4">
      <input
        ref="input"
        class="bg-transparent border-2 rounded-xl border-red-500 px-3 py-3"
        type="text"
        placeholder="Add new todo"
      />
    </form>
    <ul class="flex flex-col gap-y-6">
      <li
        class="flex justify-between px-4 py-3 rounded-lg border-red-500 border-2 gap-x-44 items-center"
        v-for="(item, index) in todos"
        :key="index"
      >
        <span
          class="truncate max-w-[10rem]"
          :class="item.isCompleted && 'line-through'"
          >{{ item.text }}</span
        >
        <div class="flex gap-x-2">
          <button
            class="bg-red-500 rounded-sm px-3 py-2"
            @click="toggleTodo(index)"
          >
            {{ item.isCompleted ? "Finished" : "Not Finished" }}
          </button>
          <button
            @click="deleteTodo(index)"
            class="w-10 h-10 bg-red-500 rounded-full flex items-center justify-center"
          >
            x
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>
