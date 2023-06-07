<template>
  <h1>Welcome To Voting App</h1>
  <ul>
      <li v-for="item,index in items" :key="item.id">
        <img :src="item.imgUrl" alt="img" style="width: 200px;"/>
        <p>Vote: {{ item.vote }}</p>
        <button @click="vote(index)">Vote</button>
      </li>
    </ul>
    <button type="button" @click="logout()">Logout</button>
</template>

<script>
  import data from '../assets/data'
  
  export default {
  data() {
    return {
      items: data,
      isVoted: false,
      lastIndex: null
    };
  },
  beforeCreate() {
    const authUser = this.$cookies.get('auth-user');
    if (authUser === null) {
      window.location.href = '/login'
    }
  },
  methods: {
    vote(index) {
      if (this.isVoted){
        if (this.lastIndex === index){
          this.items[this.lastIndex].vote--;
          this.lastIndex = null
        } else {
          if (this.lastIndex !== null){
            this.items[this.lastIndex].vote--;
          }
          this.items[index].vote++;
          this.lastIndex = index;
        }
      } else {
        this.isVoted = true
        this.items[index].vote++;
        this.lastIndex = index
      }
    },
    logout() {
      this.$cookies.remove('auth-user')
      window.location.href = '/login'
    }
  },
};
</script>