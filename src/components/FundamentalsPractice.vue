<script setup>
import { ref, reactive, onMounted } from 'vue';

const name = ref("Muzammil Ali");
const tempObj = reactive({
  task1: "Shower",
  task2: "Eat",
  task3: "Sleep"
})
const link = ref("https://www.google.com");
const newTask = ref('');

const tasks = ref(["Task 1", "Task 2", "Task 3"]);

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos?_limit=5");
    const data = await response.json();
    tasks.value = data.map((todo) => todo.title);
  } catch (error) {
    console.log("Error Fetching Data");
  }
});

</script>

<template>
  <h1>Vue Jobs {{ name }}</h1>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks :</h3>
  <ul>
    <li v-for="(value, key) in tasks" :key="key">
      <span>{{ value }}</span>
      <button @click="deleteTask(key)">Delete</button>
    </li>
  </ul>

  <a :href="link">Click for Google</a>
  <br />
  <button @click="console.log('Hello')">Change Status</button>

</template>

<style scoped>
h1 {
  color: red;
}
</style>
