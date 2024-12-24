<template>
  <div
    class="d-flex flex-md-row flex-column align-items-center justify-content-center mb-md-5 mb-2 bg-light"
  >
    <div class="input-group m-md-3 px-2">
      <input
        v-model="newTask"
        type="text"
        class="form-control"
        aria-label="Sizing example input"
        placeholder="Write your goal!"
        @keyup.enter="addNewTask"
        style="border-right: 0px; outline: 0px"
      />
      <button class="btn border border-start-0" @click="cancelNewTask" :disabled="!newTask.length">
        <i class="bi bi-x"></i>
      </button>

    </div>
    <div class="justify-content-center d-flex flex-row m-2">
      <button
        @click="addNewTask"
        :disabled="!newTask.length"
        type="button"
        class="btn btn-info me-1 text-nowrap col"
      >
        Add Item
      </button>
      <button
        @click="deleteAllTasks"
        :disabled="!tasks.length"
        type="button"
        class="btn btn-danger text-nowrap col"
      >
        Delete List
      </button>
    </div>
    
  </div>

  <div class="d-flex" v-for="(task, i) in tasks" :key="i">
    <div class="form-check ms-3 mt-1 col">
      <input
        class="form-check-input"
        style="width: 20px; height: 25px"
        type="checkbox"
        v-model="checkedTask[i]"
      />
      <p
        :class="[
          'border border-2 text-break justify-content-center p-md-2 p-1 fs-6',
          checkedTask[i] ? 'text-decoration-line-through' : ''
        ]"
      >
        {{ task }}
      </p>
    </div>
    <div>
      <button @click="editTask(i)" type="button" class="btn btn-outline-warning ms-md-3">
        <i class="bi bi-pencil-square" />
      </button>
      <button @click="deleteTask(i)" type="button" class="btn btn-outline-danger mx-md-3">
        <i class="bi bi-trash"></i>
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const newTask = ref('')
const tasks = ref<string[]>([])
let isEditing = ref(false)
let selectedIndex = ref(-1)
let checkedTask = ref([])

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

const editTask = (i: number) => {
  newTask.value = tasks.value[i] // array -> input
  isEditing.value = true
  selectedIndex.value = i
}

const deleteTask = (i:number) => {
  tasks.value.splice(i, 1)
}

const deleteAllTasks = () => {
  tasks.value.splice(0)
}

const cancelNewTask = () => {
  newTask.value = ''
}
</script>
