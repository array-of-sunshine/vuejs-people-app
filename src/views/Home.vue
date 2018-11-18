<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="person in people">
      <h3>{{ person.name }}</h3>
      <h4>{{ person.bio }}</h4>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      people: [
        {
          name: "Andy",
          bio: "my name is andy.",
          bioVisible: true
        },
        {
          name: "Alice",
          bio: "she is a person.",
          bioVisible: true
        },
        {
          name: "Buck",
          bio: "i like money.",
          bioVisible: true
        }
      ]
    };
  },
  created: function() {
    console.log('in the created function');
    console.log(this)
    axios.get('http://localhost:3000/api/people').then(function(response) {
      console.log(response.data);
      this.people = response.data
    }.bind(this))
  },
  methods: {},
  computed: {}
};
</script>
