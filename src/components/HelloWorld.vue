<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="greetings text-center">
    <h1 class="text-success">{{ msg }}</h1>
    <h3>
      Manage your Tasks easily.
     </h3>
     <p>This app is made using Vue 3 and Laravel</p>
  </div>
</template>

<style scoped>

</style>
