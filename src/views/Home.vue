<template>
  <div class="home">
    <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>|
    <router-link to="/register">Register</router-link> |
    <router-link to="/login">Login</router-link>
    <router-view></router-view>
  </div>
    <img alt="Vue logo" src="../assets/logo.png">
    <h3 v-if="user">Hi, {{user.first_name}} {{user.last_name}}</h3>
    <HelloWorld v-else msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  props: ['user']
}
</script>
