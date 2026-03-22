<script setup>
import { ref, onMounted } from "vue";

let csrf_token = ref("");
let errors = ref([]);
let successMessage = ref("");

function getCsrfToken() {
  fetch("/api/v1/csrf-token")
    .then((response) => response.json())
    .then((data) => {
      csrf_token.value = data.csrf_token;
    });
}

function saveMovie() {
  let movieForm = document.getElementById("movieForm");
  let form_data = new FormData(movieForm);

  form_data.append('csrf_token', csrf_token.value);

  fetch("/api/v1/movies", {
    method: "POST",
    body: form_data,
    headers: {
      "X-CSRFToken": csrf_token.value,
    },
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.errors) {
        // Bonus: show errors on page
        errors.value = data.errors;
        successMessage.value = "";
      } else {
        // Bonus: show success message on page
        successMessage.value = "Movie Successfully Added!";
        errors.value = [];
        movieForm.reset();
      }
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  getCsrfToken();
});
</script>

<template>
  <div class="container mt-4">
    <h2>Upload Form</h2>

    <!-- Bonus: Success Message -->
    <div v-if="successMessage" class="alert alert-success">
      {{ successMessage }}
    </div>

    <!-- Bonus: Error Messages -->
    <div v-if="errors.length > 0" class="alert alert-danger">
      <ul>
        <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
      </ul>
    </div>

    <form id="movieForm" @submit.prevent="saveMovie" enctype="multipart/form-data">
      <div class="form-group mb-3">
        <label for="title" class="form-label">Movie Title</label>
        <input type="text" name="title" id="title" class="form-control" />
      </div>

      <div class="form-group mb-3">
        <label for="description" class="form-label">Description</label>
        <textarea name="description" id="description" class="form-control" rows="4"></textarea>
      </div>

      <div class="form-group mb-3">
        <label for="poster" class="form-label">Photo Upload</label>
        <input type="file" name="poster" id="poster" class="form-control" accept="image/*" />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>