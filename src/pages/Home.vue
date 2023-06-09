<template>
  <div v-cloak class="home-container">
    <h1 class="home-heading">Welcome To Voting App</h1>
    <ul class="home-list-container">
      <li class="list" v-for="item,index in items" :key="item.id">
        <img :src="item.imgUrl" alt="img" class="img"/>
        <p class="vote-text">Vote: {{ item.vote }}</p>
        <button class="vote-btn" @click="vote(index)">Vote</button>
      </li>
    </ul>
    <div class="button-container">
      <button type="button" class="btn save-btn" @click="save()">Save</button>
      <button type="button" class="btn reset-btn" @click="reset()">Reset Vote</button>
      <button type="button" class="btn logout-btn" @click="logout()">Logout</button>
  </div>
</div>
</template>

<script>
  import axios from 'axios'
  import data from '../assets/data'
  

  const getData = () => {
    const Votingdata = localStorage.getItem('votingData')
    if (Votingdata === null){
      return data
    } else {
      const parsedData = JSON.parse(Votingdata)
      return parsedData
    } 
  }

  const getDbData = () => {
    
  }

  const getVoteSts = () => {
    const userId = $cookies.get('auth-user')
    
    const userStats = localStorage.getItem(`user${userId}`)
    if (userStats === null){
      return false 
    } else {
      const parsedData = JSON.parse(userStats)
      return parsedData.voteStats
    }
  }

  const getLastIndex = () => {
    const userId = $cookies.get('auth-user')

    const userStats = localStorage.getItem(`user${userId}`)
    if (userStats === null){
      return null 
    } else {
      const parsedData = JSON.parse(userStats)
      return parsedData.indexStats
    }
  }
  
  export default {
  data() {
    return {
      items: null, //getData(),
      isVoted: getVoteSts(),
      lastIndex: getLastIndex()
    };
  },
  beforeMount() {
    const authUser = this.$cookies.get('auth-user');
    if (authUser === null) {
      window.location.href = '/login'
    }
  },
  created() {
    axios.get('http://localhost:3000/data',)
  .then(response => {
    this.items =  response.data
  })
  .catch(error => {
    console.error(error);
  });
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
    save() {
      const votingData = this.items
      const jsonData = JSON.stringify(votingData)
      localStorage.setItem('votingData', jsonData)

      const userid = this.$cookies.get('auth-user')
      const userStats = {userId: userid, voteStats: this.isVoted, indexStats: this.lastIndex}
      const userstatsJson = JSON.stringify(userStats)
      localStorage.setItem(`user${userid}`, userstatsJson)
      console.log("saved")
    },
    reset() {
      localStorage.clear();
      this.isVoted = false
    },
    logout() {
      this.$cookies.remove('auth-user')
      window.location.href = '/login'
    }
  },
};
</script>

<style scoped src="../assets/css/home.css">
/* @import '../assets/css/home.css'; */
</style>