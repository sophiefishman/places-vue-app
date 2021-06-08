<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h1>{{ place.name }}</h1>
      <h3>{{ place.address }}</h3>
    </div>
    <div>
      <input type="text" v-model="newPlaceParams.name" placeholder="Name" />
      <input type="text" v-model="newPlaceParams.address" placeholder="Address" />

      <button v-on:click="createPlaces()">Create Place</button>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
      newPlaceParams: {},
      showPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },

    createPlaces: function () {
      axios.post("http://localhost:3000/places", this.newPlaceParams).then((response) => {
        console.log(response.data);
        this.places.push(response.data);
      });
    },
    updatePlace: function () {
      axios.patch(`http://localhost:3000/places/${this.currentPlace.id}`, this.currentPlace).then((response) => {
        console.log(response.data);
        this.currentPlace = {};
      });
    },
    destroyPlace: function () {
      axios.delete(`http://localhost:3000/places/${this.currentPlace.id}`).then((response) => {
        var index = this.places.indexOf(this.currentPlace);
        this.places.splice(index, 1);
        console.log(response.data);
      });
    },
  },
};
</script>
