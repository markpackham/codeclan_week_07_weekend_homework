<template>
  <div>
    <h2>Choose a film</h2>
    <div>
      <form v-on:submit.prevent>
        <input
          class="search"
          type="text"
          v-model="search"
          placeholder="Search for a film"
          v-on:keyup="searchForFilm"
        />
      </form>
      <br />
      <select v-on:change="handleSelect" v-model="selectedFilm">
        <option v-for="(film, index) in films" :key="index" :value="film">{{film.title}}</option>
      </select>
    </div>

    <div v-if="selectedFilm">
      <button
        v-if="!favFilms.includes(selectedFilm)"
        v-on:click="addToFav"
      >Add: {{selectedFilm.title}} to favourites</button>

      <h4>Favourite films list</h4>
      <ul>
        <li v-for="(film, index) in favFilms" :film="film" :key="index">
          {{film.title}}
          <button
            class="btn-danger"
            v-on:click="removeFav(film)"
          >Remove: {{film.title}}</button>
        </li>
      </ul>
      <button class="btn-danger" v-on:click="removeAllFavs()">Remove All Favs</button>
    </div>
  </div>
</template>

<script>
import { eventBus } from "../main.js";

export default {
  name: "main-filter",
  data() {
    return {
      search: "",
      selectedFilm: {},
      favFilms: []
    };
  },
  props: ["films"],
  methods: {
    searchForFilm() {
      let foundFilm = this.films.find(film => {
        return film.title.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
      });
      this.selectedFilm = foundFilm;
      eventBus.$emit("film-selected", this.selectedFilm);
    },
    handleSelect() {
      eventBus.$emit("film-selected", this.selectedFilm);
    },
    addToFav() {
      this.favFilms.push(this.selectedFilm);
    },
    removeFav(film) {
      this.favFilms.splice(this.favFilms.indexOf(film), 1);
    },
    removeAllFavs() {
      this.favFilms = [];
    }
  }
};
</script>

<style>
</style>