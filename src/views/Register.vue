<template>
    <h1>Register form</h1>
    <form @submit.prevent="register">
        <input type="text" placeholder="First name" v-model="first_name" /><br>
        <input type="text" placeholder="Last name" v-model="last_name" /><br>
        <input type="email" placeholder="Email" v-model="email" /><br>
        <input type="password" placeholder="password" v-model="password" /><br>
        <input type="password" placeholder="confirm password" v-model="confirm_password" /><br>
        <button>Sign Up</button>

    </form>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Register',
  data() {
    return {
      first_name:'',
      last_name:'',
      email:'',
      password:'',
      confirm_password:''
    }
  },
  methods: {
    async register(){
     const response = await axios.post('register',{
        first_name : this.first_name,
        last_name : this.last_name,
        email : this.email,
        password : this.password,
        confirm_password : this.confirm_password
      });
      console.log(response);
      this.$router.push('/login')
    }
  }
}
</script>