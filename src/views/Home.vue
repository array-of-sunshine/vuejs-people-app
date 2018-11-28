<template>
  <div class="home">
    <h1>{{ people.length }} people</h1>
    Search by name or bio: <input v-model="nameFilter" list="names">
    <datalist id="names">
      <span v-for="person in people">
        <option>{{ person.name }}</option>
        <option>{{ person.bio }}</option>
      </span>
    </datalist>
    <div>
      <button @click="setSortAttribute('name')">Sort by name</button>
      <button @click="setSortAttribute('bio')">Sort by bio</button>
    </div>

    <!-- <div v-for="person in filterBy(people, nameFilter)"> -->
    <transition-group name="purple-hippo">
      <div v-for="person in orderBy(people, sortAttribute, sortAscending)" :key="person.id">
      <!-- <div v-for="person in orderBy(filterBy(people, nameFilter, 'name', 'bio'), sortAttribute)"> -->
        <h4 v-on:click="toggleBioVisible(person)">{{ person.name }}</h4>
        <h3 v-bind:class="{red: person.bioVisible, blue: !person.bioVisible}">{{ person.bio }}</h3>

        <button v-on:click="removePerson(person)">Remove this person</button>
        <hr>
      </div>
    </transition-group>

    <p class="red" v-for="error in errors">{{error}}</p>
    <p>name:<input type="text" v-model="newPerson.name"></p>
    <p>bio:<input type="text" v-model="newPerson.bio"></p>
    <button v-on:click="addPerson()">add a new person</button>
  </div>
</template>

<style>
.purple-hippo-red {
  color: red;
}
.blue {
  color: blue;
}

/* Vue.js fade */
.fade-enter-active, .fade-leave-active, .purple-hippo-enter-active, .purple-hippo-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-to, .purple-hippo-enter, .purple-hippo-leave-to {
  opacity: 0
}

/* Vue.js slide-right */
.slide-right-enter-active {
  transition: all .3s ease;
}
.slide-right-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-right-enter, .slide-right-leave-to {
  transform: translateX(10px);
  opacity: 0;
}

/* Vue.js slide-left */
.slide-left-enter-active {
  transition: all .3s ease;
}
.slide-left-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-left-enter, .slide-left-leave-to {
  transform: translateX(-10px);
  opacity: 0;
}

</style>

<script>
var axios = require('axios');
// import axios from 'axios'
import Vue2Filters from 'vue2-filters'
export default {
  mixins: [Vue2Filters.mixin], 
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      people: [],
      newPerson: { name: "", bio: "", bioVisible: true },
      errors: [],
      nameFilter: '',
      sortAttribute: 'name',
      sortAscending: 1
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
        this.errors = [];
      }.bind(this)).catch(function(errors) {
        console.log('in the .catch function');
        console.log(errors.response);
        // what should i do in the .catch?
        // tell the user they did something wrong
        this.errors = errors.response.data.errors;
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
    },
    setSortAttribute: function(inputAttribute) {
      // if (this.sortAscending === 1) {
      //   this.sortAscending = -1;
      // } else { // if this.sortAscending === -1
      //   this.sortAscending = 1;
      // }
      if (this.sortAttribute === inputAttribute) {
        this.sortAscending *= -1;
      } else {
        this.sortAttribute = inputAttribute;
      }
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
