<template>
  <div class="home">
    <h1>{{ people.length }} people</h1>
    <div v-for="person in people">
      <h4>{{ person.name }}</h4>
      <h3>{{ person.bio }}</h3>
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

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      people: [
        { name: "alice", bio: "my name is alice", bioVisible: true },
        { name: "barney", bio: "i like to go to the beach", bioVisible: true },
        { name: "charlie", bio: "i like checkers", bioVisible: true }
      ],
      newPerson: { name: "darlene", bio: "my name is darlene", bioVisible: true }
    };
  },
  created: function() {
  },
  methods: {
    addPerson: function() {
      console.log('add the person');
      this.people.push(this.newPerson);
    },
    removePerson: function(inputPerson) {
      console.log('remoing the person');
      console.log(inputPerson);
      // find where inputPerson is
      var index = this.people.indexOf(inputPerson);
      // remove inputPerson
      this.people.splice(index, 1);
    }
  },
  computed: {}
};

// click a button to run a function
// that function adds a new person to the people array
// user dynamic input to change values of person
</script>
