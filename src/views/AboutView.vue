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

    <div class="row">
      <div
        class="col-md-6 mb-4"
        v-for="movie in movies"
        :key="movie.id"
      >
        <div class="card">
          <div class="row g-0">
            <div class="col-4">
              <img
                :src="movie.poster"
                :alt="movie.title"
                class="img-fluid rounded-start"
                style="height: 100%; object-fit: cover;"
              />
            </div>
            <div class="col-8">
              <div class="card-body">
                <h5 class="card-title">{{ movie.title }}</h5>
                <p class="card-text">{{ movie.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Show message if no movies exist yet -->
    <p v-if="movies.length === 0">No movies added yet.</p>
  </div>
</template>

<style>
</style>
