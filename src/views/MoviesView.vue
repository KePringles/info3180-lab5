<script setup>
import { ref, onMounted } from "vue";
let movies = ref([]);
function fetchMovies() {
  fetch("/api/v1/movies", {
    method: "GET",
  })
    .then((response) => response.json())
    .then((data) => {
      movies.value = data.movies;
    })
    .catch((error) => {
      console.log(error);
    });
}
onMounted(() => {
  fetchMovies();
});
</script>

<template>
    <div class="container mt-4">
      <h2>Movies</h2>
      <div class="row row-cols-1 row-cols-md-2 g-3">
        <div
          class="col"
          v-for="movie in movies"
          :key="movie.id"
        >
          <div class="card h-100">
            <div class="row g-0 h-100">
              <div class="col-4" style="overflow: hidden;">
                <img
                  :src="movie.poster"
                  :alt="movie.title"
                  style="width: 100%; height: 100%; object-fit: cover; object-position: top;"
                />
              </div>
              <div class="col-8">
                <div class="card-body">
                  <h5 class="card-title">{{ movie.title }}</h5>
                  <p class="card-text" style="font-size: 0.85rem;">{{ movie.description }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <p v-if="movies.length === 0">No movies added yet.</p>
    </div>
  </template>

<style>
</style>