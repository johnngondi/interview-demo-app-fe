<script setup>
import { onMounted, ref } from 'vue';
import { API_URL } from '@/constants';
import axios from 'axios';

const loggedIn = ref(false);
const loggedInAs = ref('');

const email = ref('');
const password = ref('');

function checkLogin() {

  const token = sessionStorage.getItem("i_d_a_token");

  if (token) {
    loggedIn.value = true;
    loggedInAs.value = sessionStorage.getItem("i_d_a_user")

    //Validate token
    axios.get(API_URL + '/login-status', {
      headers: {
        'Authorization': 'Bearer ' + token
      }
    })
      .then(function (response) {
        // handle success
        console.log(response);
      })
      .catch(function (error) {
        // handle error
        logoutUser()
      })

  }
}

function loginUser() {

  axios.post(API_URL + '/login',
    {
      email: email.value,
      password: password.value
    })
    .then(function ({ data }) {
      // handle success
      sessionStorage.setItem("i_d_a_user", data.data.user.name);
      sessionStorage.setItem("i_d_a_token", data.data.token);


      loggedIn.value = true;

    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })

}

function logoutUser() {
  const token = sessionStorage.getItem("i_d_a_token");

  axios.post(API_URL + '/logout', {}, {
    headers: {
      'Authorization': 'Bearer ' + token
    }
  })
    .then(function (response) {
      // handle success
      console.log(response);
      sessionStorage.removeItem('i_d_a_user');
      sessionStorage.removeItem('i_d_a_token');
      loggedIn.value = false;
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
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
