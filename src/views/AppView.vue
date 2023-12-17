
<script setup>
import router from "@/router";
import { ref, onMounted } from "vue";
import { API_URL } from '@/constants';
import axios from 'axios';

const tasks = ref([]);

const taskTitle = ref('');
const hasError = ref(false);
const canCreateTask = ref(false);
const sending = ref(false);
const token = ref('');

function checkLogin() {
  const storedToken = sessionStorage.getItem("i_d_a_token");

  if (storedToken) {
    //Validate token
    token.value = storedToken;
    axios.get(API_URL + '/login-status', {
      headers: {
        'Authorization': 'Bearer ' + storedToken
      }
    })
      .then(function ({data}) {
        // handle success
        canCreateTask.value = data.can_create_tasks;

      })
      .catch(function (error) {
        // handle error
        router.push({ name: 'home' })
      })

  } else {
    router.push({ name: 'home' })
  }
}

function getTasks() {
  axios.get(API_URL + '/tasks', {
      headers: {
        'Authorization': 'Bearer ' + token.value
      }
    })
      .then(function ({data}) {

        console.log(data.data);
        
        tasks.value = data.data;

      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
}

function addTask() {
  hasError.value = false;

  if (!taskTitle.value) {
    hasError.value = true;
    return;
  }

  sending.value = true;

  axios.post(API_URL + '/tasks', {title: taskTitle.value}, {
      headers: {
        'Authorization': 'Bearer ' + token.value
      }
    })
      .then(function ({data}) {

        tasks.value.push(data.data);
        
        taskTitle.value = '';

      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
      .finally(function(){
        sending.value = false;
      })


}

function removeTask(id, index) {
  tasks.value.splice(index, 1);

  axios.delete(API_URL + '/tasks/' + id, {
      headers: {
        'Authorization': 'Bearer ' + token.value
      }
    })
      .then(function (response) {

        console.log(response);

      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
}

function markTaskAs(id, index, value) {

  axios.patch(API_URL + '/tasks/' + id, {status: value}, {
      headers: {
        'Authorization': 'Bearer ' + token.value
      }
    })
      .then(function (response) {

        console.log(response);

      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })

}


onMounted(() => {
  checkLogin();
  getTasks();
})

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
            <input class="form-check-input" type="checkbox" v-model="task.done" :id="'task-' + task.id"
              @change="markTaskAs(task.id, index, task.done)">
            <label class="form-check-label ps-2" :for="'task-' + task.id">
              <span :class="task.done ? 'text-decoration-line-through' : ''">
                Task#{{ task.id }}
              </span>
            </label>

          </th>
          <td>


            <span :class="task.done ? 'text-decoration-line-through' : ''">
              {{ task.title }} <br>
              <small class="text-muted">{{ task.created_at }}</small>
            </span>
          </td>
          <td>
            <button class="btn btn-danger btn-sm" @click="removeTask(task.id, index)">Remove</button>
          </td>
        </tr>
      </tbody>

      <tfoot v-if="canCreateTask">
        <tr>
          <td colspan="2">
            <input type="text" class="form-control" v-model="taskTitle" id="taskTitle" placeholder="Enter Task title">
            <div v-if="hasError" id="taskTitleHelp" class="text-danger text-sm">
              The Task Title field is required
            </div>
          </td>
          <td>
            <button type="button" class="btn btn-primary" @click="addTask" :disabled="sending">
              {{ sending ? 'Creating...' : 'Add Task' }}
            </button>
          </td>
        </tr>
      </tfoot>

    </table>

  </div>
</template>

<style></style>
