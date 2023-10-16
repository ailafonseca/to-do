<template>
  <div class="row">
    <div class="col-10">
      <div class="input-group input-group-lg mb-3">
        <input
          v-model="newTask"
          type="text"
          class="form-control"
          aria-label="Sizing example input"
          aria-describedby="inputGroup-sizing-lg"
          placeholder="Write your goal!"
          id="input"
        />
        <div class="input-group-append mx-3">
          <button @click="addNewTask" class="btn btn-info" type="button">Add</button>
          <button @click="deleteAllTasks" type="button" class="btn btn-danger ms-3">Delete List</button>
        </div>
      </div>
    </div>
    <div class="row" id="border" v-for="(task, i) in tasks" :key="i">
      <div class="col">{{ task }}</div>
      <div class="col-auto">
        <button @click="editTask(i)" type="button" class="btn btn-warning"> 
          <i class="bi bi-pencil-square"/>  
        </button>
        <button @click="deleteTask(i)" type="button" class="btn btn-danger ms-3">
          <i class="bi bi-trash"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<style>

#input{
  border: 2px solid rgb(156, 156, 156);
  font-size: 20px;
  align-items: center;
  background-color: white;
  text-wrap: balance;
}

#border {
  border-top: 1px solid rgb(156, 156, 156);
  padding: 10px;
  margin: 0.04px;
  margin-left: 10px;
  background-color: white;
  width: 100%;
  text-wrap: balance;
}

</style>

<script lang="ts" setup>
import { ref } from 'vue'
const newTask = ref('')
const tasks = ref<string[]>([])
let isEditing = ref(false)
let selectedIndex = ref(-1)

const addNewTask = () => {
  if (isEditing.value) {
    tasks.value[selectedIndex.value] = newTask.value // input -> array
    newTask.value = ''
    isEditing.value = false
  } else {
    tasks.value.push(newTask.value)
    newTask.value = ''
  }
}

const editTask = (i) => {
  newTask.value = tasks.value[i] // array -> input
  isEditing.value = true
  selectedIndex.value = i
}

const deleteTask = (i) => {
  tasks.value.splice(i, 1)
}

const deleteAllTasks = () => {
  tasks.value.splice(0)
}
</script>
