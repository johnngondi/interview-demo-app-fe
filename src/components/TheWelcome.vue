<script setup>
import { onMounted, ref } from 'vue';
import { API_URL } from '@/constants';

const loggedIn = ref(false);
const loggedInAs = ref('');

const email = ref('');
const password = ref('');

function checkLogin() {

  console.log(API_URL);

  let data = sessionStorage.getItem("i_d_a_user");

  if (data) {
    loggedIn.value = true;
    loggedInAs.value = data.name
  }
}

function loginUser() {



  console.log(email.value, password.value);
}

function logoutUser() {
  sessionStorage.removeItem('i_d_a_user');
}

onMounted(() => {
  checkLogin();
})

</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-12 text-center">

        <div v-if="loggedIn">
          <h5>You are logged in as {{ loggedInAs }}</h5>

          <RouterLink to="/app" class="btn btn-primary">
            Manage Tasks
          </RouterLink>

          or

          <button type="button" @click="logoutUser" class="btn btn-danger btn-xs">Logout</button>

        </div>

        <div v-else>

          <div class="container">
            <div class="row">
              <div class="col-12">
                <form @submit.prevent="loginUser">

                  <h5>You are not logged in. Log in to continue!</h5>

                  <div class="mb-3 text-left">
                    <label for="exampleInputEmail1" class="form-label">Email address</label>
                    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp"
                      v-model="email" required>
                    <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
                  </div>
                  <div class="mb-3 text-left">
                    <label for="exampleInputPassword1" class="form-label">Password</label>
                    <input type="password" class="form-control" id="exampleInputPassword1" v-model="password" required>
                  </div>

                  <button type="submit" class="btn btn-primary">Submit</button>
                </form>
              </div>
            </div>
          </div>

        </div>

      </div>
    </div>
  </div>
</template>
