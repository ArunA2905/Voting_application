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
  
  export default {
  data() {
    return {
      items: [],
      isVoted: false,
      lastIndex: null
    };
  },
  beforeMount() {
    const authUser = this.$cookies.get('auth-user');
    if (authUser === null) {
      window.location.href = '/login'
    }
  },
  async created() {
    axios.get('http://localhost:3000/data',)
  .then(response => {
    const data = response.data
    const arrangedData = data.sort((a, b) => a.id - b.id);
    this.items =  arrangedData
  })
  .catch(error => {
    console.error(error);
  });

  let userStatus;

  try {
    const response = await axios.get('http://localhost:3000/user_stats');
    userStatus = response.data;
  } catch (error) {
    console.error(error);
  }

  const userid = this.$cookies.get('auth-user')

  const isUserStatId = userStatus.filter(each => each.userId === parseInt(userid))
  //console.log(isUserStatId)

  if (isUserStatId.length !== 0) {
    this.isVoted = userStatus[0].voteStatus
    this.lastIndex = userStatus[0].lastVote
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
    async save() {
      this.items.forEach((item,i) => {
      const data = { vote: item.vote };
    
      axios.put(`http://localhost:3000/data/${i+1}`, data)
        .then(response => {
          //console.log(response.data);
        })
        .catch(error => {
          console.error(error);
        });
      });

      const userid = this.$cookies.get('auth-user')
      const userStats = {userId: userid, voteStatus: this.isVoted, lastVote: this.lastIndex}
        
      let userStatus;

      try {
        const response = await axios.get('http://localhost:3000/user_stats');
        userStatus = response.data;
      } catch (error) {
        console.error(error);
      }

      const userPresent = userStatus.find(eachItem => eachItem.userId === parseInt(userid))
      
      if (userPresent === undefined){
        axios.post('http://localhost:3000/user_stats', {
        user_stat: {
          ...userStats
        }
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        }).then(response => {
            //console.log(response);
          })
          .catch(error => {
            console.error(error);
          });
      } else {
        const data = {
          voteStatus: this.isVoted,
          lastVote: this.lastIndex,
        };

        axios.put(`http://localhost:3000/user_stats/${userid}`, data)
          .then(response => {
            //console.log(response.data);
          })
          .catch(error => {
            console.error(error);
          });
      }
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
</style>