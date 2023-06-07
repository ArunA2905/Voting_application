<template>
  <h1>Welcome To Voting App</h1>
  <ul>
      <li v-for="(item,index) in items" :key="item.id">
        <img :src="item.imgUrl" alt="img" style="width: 200px;"/>
        <p>Vote: {{ item.vote }}</p>
        <button @click="vote(index)">Vote</button>
      </li>
    </ul>
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
  methods: {
    vote(index) {
      if (this.isVoted){
        this.items[this.lastIndex].vote--;
        this.items[index].vote++;
        this.lastIndex = index;
      } else {
        this.isVoted = true
        this.items[index].vote++;
        this.lastIndex = index
      }
    },
  },
};
</script>