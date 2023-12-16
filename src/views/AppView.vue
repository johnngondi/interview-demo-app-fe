
<script setup>
import { ref } from "vue";

const tasks = ref([
  {
    id: 1,
    title: 'Make App',
    done: true
  },
  {
    id: 2,
    title: 'Publish App',
    done: false
  }
]);

const taskTitle = ref('');
const hasError = ref(false);

function checkLogin() {
  console.log('Checking if you are logged in or not.');
}

function addTask() {
  hasError.value = false;

  if (!taskTitle.value) {
    hasError.value = true;
    return;
  }


}

function removeTask(id, index) {
  tasks.value.splice(index, 1);
}

function markTaskAs(id, index, value) {
  
}

</script>


<template>
  <div class="">
    <h1>Manage Tasks here</h1>

    <table class="table table-hover">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Task</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="task.id">
          <th scope="row">
            <input class="form-check-input" type="checkbox" v-model="task.done" :id="'task-' + task.id" @change="markTaskAs(task.id, index, task.done)">
            <label class="form-check-label ps-2" :for="'task-' + task.id">
              <span :class="task.done ? 'text-decoration-line-through' : ''">
                Task#{{ task.id }}
              </span>
            </label>

          </th>
          <td>


            <span :class="task.done ? 'text-decoration-line-through' : ''">
              {{ task.title }}
            </span>
          </td>
          <td>
            <button class="btn btn-danger btn-sm" @click="removeTask(task.id, index)">Remove</button>
          </td>
        </tr>
      </tbody>

      <tfoot>
        <tr>
          <td colspan="2">
            <input type="text" class="form-control" v-model="taskTitle" id="taskTitle" placeholder="Enter Task title">
            <div v-if="hasError" id="taskTitleHelp" class="text-danger text-sm">
              The Task Title field is required
            </div>
          </td>
          <td>
            <button type="button" class="btn btn-primary" @click="addTask">Add Task</button>
          </td>
        </tr>
      </tfoot>

    </table>

  </div>
</template>

<style></style>
