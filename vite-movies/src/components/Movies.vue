<template>
  <div class="home">
    <div class="feature-card">
      <!---<img
        src="https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&q=80"
        alt="Movie theater"
        class="featured-img"
      />
      <div class="detail">
        <h3>Title</h3>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Nobis
          accusamus, numquam, vero accusantium ipsa magnam officiis repellendus
          eligendi id facere aperiam, error dolore. Iure, modi ea quam quae
          numquam cupiditate.
        </p>
      </div>--->

      <form @submit.prevent="SearchMovies()" class="search-box">
        <input
          type="text"
          v-model="search"
          placeholder="What are you looking for?"
        />
        <input type="submit" value="Search" />
      </form>
      <div class="popup" v-if="movieObj != null">
        <div class="popup-inner">
          <h2>{{ movieObj.Title }} - <span>{{movieObj.Rated}}</span></h2>
          <p>{{ movieObj.Plot }}</p>
          <button @click="CloseInfo()">Close</button>
        </div>
      </div>
      <div class="movies-list" v-if="movies.length > 0">
        <h1>Movies</h1>
        <div class="movie" v-for="movie in movies" :key="movie.imdbID">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="detail">
              <p class="year">{{ movie.Year }}</p>
              <h3>{{ movie.Title }}</h3>
              <button @click="GetInfo(movie.imdbID)">More</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "../env.js";
export default {
  setup() {
    const search = ref("");
    const movies = ref([]);
    let movieObj = ref();
    const CloseInfo = () => {
      movieObj.value = null;
    };
    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            search.value = "";
          });
      }
    };
    const GetInfo = (id) => {
      fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${id}`)
        .then((response) => response.json())
        .then((data) => {
          movieObj.value = data
          console.log(data)
        });
    };
    return {
      search,
      movies,
      SearchMovies,
      GetInfo,
      movieObj,
      CloseInfo,
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  justify-content: center;
  width: 100%;
}
.featured-img {
  object-fit: cover;
  width: 100%;
}
img {
  width: 100%;
}
.search-box {
  display: flex;
  flex-direction: column;
  justify-items: center;
  align-items: center;
  padding: 16px;
}
input {
  display: block;
  appearance: none;
  border: none;
  outline: none;
  background: none;
}
input[type="text"] {
  width: 50%;
  color: #fff;
  background: teal;
  padding: 10px 16px;
  font-size: 20px;
  border-radius: 8px;
  margin-bottom: 15px;
  transition: 0.4s;
}
input[type="text"]::placeholder {
  color: #f3f3f3;
}
input[type="text"]:focus {
  box-shadow: 8px 3px 6px rgba(0, 0, 0, 0.5);
}

input[type="submit"] {
  width: 50%;
  background-color: teal;
  text-transform: uppercase;
  transition: 0.4s;
  padding: 16px;
  border-radius: 8px;
}
.movies-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0 8px;
}
.movie {
  max-width: 50%;
  padding: 10px 8px;
  flex: 1 1 50%;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
}

.popup-inner {
  background: #fff;
  padding: 32px;
}
</style>
