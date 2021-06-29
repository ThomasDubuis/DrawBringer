<template>
    <router-view></router-view>
</template>

<script>
import axios from 'axios'

export default {
  name:'App',
  data() {
    return {
      user:null
    }
  },
  async created() {
    const response = await axios.get('user',{
      headers:{
        Authorization: 'Bearer ' + localStorage.getItem('token')
      }
    });
    this.user = response.data;
  }
}
</script>


<style>
* {
	box-sizing: border-box;
}
body {
  margin:0;
  padding:0;
  height: 100vh;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
