<template>
  <h1>Welcome To Voting App</h1>
  <ul>
    <li v-for="item,index in items" :key="item.id">
      <img :src="item.imgUrl" alt="img" style="width: 200px;"/>
      <p>Vote: {{ item.vote }}</p>
      <button @click="vote(index)">Vote</button>
    </li>
  </ul>
  <button type="button" @click="save()">Save</button>
  <button type="button" @click="reset()">Reset Vote</button>
  <button type="button" @click="logout()">Logout</button>
</template>

<script>
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

  const getVoteSts = () => {
    const userName = $cookies.get('auth-user')
    
    const userStats = localStorage.getItem(`user${userName}`)
    if (userStats === null){
      return false 
    } else {
      const parsedData = JSON.parse(userStats)
      return parsedData.voteStats
    }
  }

  const getLastIndex = () => {
    const userName = $cookies.get('auth-user')

    const userStats = localStorage.getItem(`user${userName}`)
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
      items: getData(),
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

      const userName = this.$cookies.get('auth-user')
      const userStats = {username: userName, voteStats: this.isVoted, indexStats: this.lastIndex}
      const userstatsJson = JSON.stringify(userStats)
      localStorage.setItem(`user${userName}`, userstatsJson)
    },
    reset() {
      localStorage.clear();
    },
    logout() {
      this.$cookies.remove('auth-user')
      window.location.href = '/login'
    }
  },
};
</script>