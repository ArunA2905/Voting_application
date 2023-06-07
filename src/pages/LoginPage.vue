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
      return false 
    } return true
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
  methods: {
    login() {
      if (this.email !== '' && this.password !== '') {
        this.err = false
        const authentication = getAuthentication(this.email, this.password, credential)
        if (authentication){
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
