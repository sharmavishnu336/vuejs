<template>
  <div class="app">
  <div class="container">
    <input type="text" placeholder="Type your query" v-model="search" @blur="toggle = false" @focus="toggle = true">
    <div class="results" v-if="toggle">
      <div class="result" v-for="user in newUsers" :key="user.id">
        <span @click="selectResult(user)" @mousedown.prevent>
          {{ user.name }}
        </span>
      </div>
    </div>
  </div>
</div>


</template>

<script>
import axios from 'axios';

export default {
  name: 'SearchAutocomplete',
  data() {
    return {
     search: '',
      toggle: false,
      users: [],
      newUsers: []
    };
  },
  mounted () {
    axios.get('http://127.0.0.1:8000/api/getUser')
      .then(response =>
      this.users = response.data.user)
  },

  watch: {
    search(val) {
      this.newUsers = [];
      this.users.map(element => {
        if(element.name.includes(val) && val != "") {
          this.newUsers.push(element);
        }
      });
    }
  },
 methods: {
    selectResult(user) {
      this.search = user.name;
    }
  }
  
}
</script>