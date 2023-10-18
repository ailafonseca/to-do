<template>
  <div class="row" id="center">
    <div class="col-10">
      <div class="input-group mb-3">
        <input
          v-model="newTask"
          type="text"
          class="form-control"
          aria-label="Sizing example input"
          aria-describedby="inputGroup-sizing-lg"
          placeholder="Write your goal!"
          id="input"
          @keyup.enter="addNewTask"
        />
        <div class="input-group-append mx-3">
          <button @click="addNewTask" :disabled="!newTask.length" class="btn btn-info" type="button">Add</button>
          <button @click="deleteAllTasks" :disabled="!tasks.length" type="button" class="btn btn-danger ms-3">Delete List</button>
        </div>
      </div>
    </div>
    <div class="row" v-for="(task, i) in tasks" :key="i">
      <div class="col" id="border">{{ task }}</div>
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

#center{
  display: flex;
  justify-content: center;
}

#input{
  border: 2px solid rgb(156, 156, 156);
  font-size: 20px;
  align-items: center;
  background-color: white;
  text-wrap: balance;
}

#border {
  border: 1px solid rgb(156, 156, 156);
  padding: 10px;
  margin: 0.04px;
  margin-left: 10px;
  background-color: white;
  max-width: 100%;
  inline-size: 300px;
  overflow-wrap: break-word;
}

</style>

<script lang="ts" setup>
import { ref } from 'vue'
const newTask = ref('')
const tasks = ref<string[]>([])
let isEditing = ref(false)
let selectedIndex = ref(-1)


const addNewTask = () => {
  //if (newTask.value.length < 1) return
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
