<template>
  <div class="login-form-container">
    <h1 class="login-heading">Login to continue</h1>
    <form @submit.prevent="login">
      <div class="input-card">
        <label class="input-label" for="email">Email : </label>
        <input class="input-bar" type="email" id="email" v-model="email">
      </div>
      <div class="input-card">
        <label class="input-label" for="password">Password : </label>
        <input class="input-bar" type="password" id="password" v-model="password">
      </div>
      <button class="login-btn" type="submit">Login</button>
      <p v-if="err">{{errMsg}}</p>
    </form>
  </div>
</template>
<script>

import credential from '../LoginCredentials/credential'

// const authUser = this.$cookies.get("auth-user");
// console.log(authUser)



const renderFailureResponse = (email,password,errMsg) => {
        if (email === '' && password === ''){
          errMsg = 'Please enter email and password'
        } else if (email === '') {
          errMsg = 'Please enter valid email'
        } else {
          errMsg = 'Please enter valid password'
        }
        return errMsg
}

const getAuthentication = (email,password, credential) => {
    const auth = credential.find(
          (cred) => cred.email === email && cred.password === password
        );
    if (auth === undefined){
      return [false, null]
    } return [true,auth.id]
}

export default {
  data() {
    return {
      email: '',
      password: '',
      err: false,
      errMsg: ''
    };
  },
  beforeMount() {
    const authUser = this.$cookies.get('auth-user');
    if (authUser) {
      window.location.href = '#/'
    }
  },
  methods: {
    login() {
      if (this.email !== '' && this.password !== '') {
        this.err = false
        const [authentication,auth] = getAuthentication(this.email, this.password, credential)

        if (authentication){
          this.$cookies.set('auth-user',auth)
          window.location.href = '#/'
        } else {
          this.err = true 
          this.errMsg = "Enter Correct Credentials...!"
        }
      } else {
        this.err = true
        this.errMsg = renderFailureResponse(this.email,this.password,this.errMsg)
      }
    },
  },
};
</script>

<style scoped src="../assets/css/login.css">
</style>