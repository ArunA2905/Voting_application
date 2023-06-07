<template>
  <div>
    <h1>Login</h1>
    <form @submit.prevent="login">
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email">
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="password">
      </div>
      <button type="submit">Login</button>
      <p v-if="err">{{errMsg}}</p>
    </form>
  </div>
</template>

<script>
import credential from '../LoginCredentials/credential'
console.log(credential)
export default {
  data() {
    return {
      email: '',
      password: '',
      err: false,
      errMsg: ''
    };
  },
  methods: {
    login() {
      if (this.email !== '' && this.password !== '') {
        this.err = false
        console.log('Login successful');
      } else {
        this.err = true
        if (this.email === '' && this.password === ''){
          this.errMsg = 'Please enter email and password'
        } else if (this.email === '') {
          this.errMsg = 'Please enter valid email'
        } else {
          this.errMsg = 'Please enter valid password'
        }
        console.log('Please enter email and password');
      }
    },
  },
};
</script>
