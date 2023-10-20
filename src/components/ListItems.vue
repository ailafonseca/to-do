<template>
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
    <button
      class="input-group-text"
      @click="cancelNewTask"
      :disabled="!newTask.length"
      id="cancel-button"
    >
      <i class="bi bi-x-square-fill"></i>
    </button>
    <div class="input-group-append mx-3">
      <button @click="addNewTask" :disabled="!newTask.length" class="btn btn-info" type="button">
        Add
      </button>
      <button
        @click="deleteAllTasks"
        :disabled="!tasks.length"
        type="button"
        class="btn btn-danger ms-3"
      >
        Delete All Goals
      </button>
    </div>
  </div>

  <div class="d-flex m-2" v-for="(task, i) in tasks" :key="i">
    <div class="form-check col">
      <input class="form-check-input" type="checkbox" v-model="checkedTask[i]" />
      <p :class="['border border-2 text-break', checkedTask[i] ? 'text-decoration-line-through' : '']">{{ task }}</p>
    </div>
    <button @click="editTask(i)" type="button" class="btn btn-warning">
      <i class="bi bi-pencil-square" />
    </button>
    <button @click="deleteTask(i)" type="button" class="btn btn-danger ms-3">
      <i class="bi bi-trash"></i>
    </button>
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

const cancelNewTask = () => {
  newTask.value = ''
}
</script>
