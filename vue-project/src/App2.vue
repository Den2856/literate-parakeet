<script setup>
import { ref, onMounted } from "vue";

const name = ref('Deniel');
const status = ref('active');
const tasks = ref(['Task one', 'Task two', 'Task three']);
const newTask = ref('new');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  }
  else if (status.value === 'pending') {
    status.value = 'inactive';
  }
  else{
    status.value = 'active';
  }
}

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos')
    const data = await response.json();
    tasks.value = data.map((task) => task.title)
  } catch (error) {
    console.log('Error fetching')
  }
});
</script>

<template>
  <h1>{{ name }}</h1> 
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inacrive</p>

  <form @submit.prevent="addTask">
    <label for="newTask"></label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span> {{ task }} </span>
      <button @click="deleteTask()">X</button>
    </li>
  </ul>
  <br>
  <!-- <button v-on:click="toggleStatus">Change status</button> -->
  <button @click="toggleStatus">Change status</button>
</template>

<style>

</style>