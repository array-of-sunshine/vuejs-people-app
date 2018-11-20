<template>
  <div class="home">
    <h1>{{ people.length }} people</h1>
    <div v-for="person in people">
      <h4 v-on:click="toggleBioVisible(person)">{{ person.name }}</h4>
      <h3 v-if="person.bioVisible">{{ person.bio }}</h3>

      <button v-on:click="removePerson(person)">Remove this person</button>
      <hr>
    </div>
    <p>name:<input type="text" v-model="newPerson.name"></p>
    <p>bio:<input type="text" v-model="newPerson.bio"></p>
    <button v-on:click="addPerson()">add a new person</button>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');
// import axios from 'axios'
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      people: [],
      newPerson: { name: "darlene", bio: "my name is darlene", bioVisible: true }
    };
  },
  created: function() {
    axios.get('http://localhost:3000/api/people').then(function(response) {
      console.log(response.data);
      this.people = response.data;
    }.bind(this))
  },
  methods: {
    addPerson: function() {
      var params = {
        name: this.newPerson.name,
        bio: this.newPerson.bio
      }
      axios.post('http://localhost:3000/api/people', params).then(function(response) {
        console.log(response.data);
        this.people.push(response.data);
      }.bind(this))
      console.log('add the person');
    },
    removePerson: function(inputPerson) {
      console.log('remoing the person');
      console.log(inputPerson);
      // find where inputPerson is
      var index = this.people.indexOf(inputPerson);
      // remove inputPerson
      this.people.splice(index, 1);
    },
    toggleBioVisible: function(inputPerson) {
      console.log('toggling bioVisible');
      console.log(inputPerson);
      inputPerson.bioVisible = !inputPerson.bioVisible;
      // if (inputPerson.bioVisible) {
      //   inputPerson.bioVisible = false;
      // } else {
      //   inputPerson.bioVisible = true;
      // }
    }
  },
  computed: {}
};

// click a button to run a function
// that function adds a new person to the people array
// user dynamic input to change values of person

// toggle bio when name is clicked
// click the name, run a function
// that function changes the bioVisible attribute of that person
// somehow make it so that when bioVisible changes, what the user sees changes
</script>
