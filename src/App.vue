<script setup>
import { ref } from "vue";
import nullImg from "./assets/null.webp";

const key = import.meta.env.VITE_MDB_API_KEY;
const currentPage = ref(1);
const total_pages = ref(null);
const movies = ref([]);
const total_results = ref(null);

const fetchMovies = async (query) => {
  try {
    const response = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${key}&query=${query.target[0].value}`);
    const data = await response.json();
    movies.value = data.results;
    currentPage.value = data.page;
    total_pages.value = data.total_pages;
    total_results.value = data.total_results;
    console.log(data);
  } catch (error) {
    console.error(error);
  }
};
</script>
<template>
  <header class="max-w-5xl mx-auto my-12">
    <h1 class="text-4xl font-mono font-bold">The Movie DB</h1>
  </header>
  <main class="max-w-5xl mx-auto m-y-12 font-mono">
    <h2 class="text-2xl">Rechercher un film</h2>
    <form class="flex my-4 space-x-2" @submit.prevent="fetchMovies">
      <input type="text" placeholder="taper ici" class="input input-bordered w-full max-w-xs" />
      <button class="btn btn-primary" type="submit">Rechercher</button>
    </form>
    <div class="divider" />
    <div v-if="total_results">
      <h2 class="text-2xl">Il y a {{ total_results }} résultats</h2>
      <div class="join" v-for="page in pages">
        <button class="join-item btn"></button>
      </div>
      <ul class="list-none grid grid-cols-2">
        <li v-for="movie in movies" :key="movie.id" class="my-4">
          <div class="card card-side overflow-hidden bg-base-100 shadow-xl">
            <img class="w-1/3" :src="movie.poster_path ? `https://image.tmdb.org/t/p/w200/${movie.poster_path}` : nullImg" :alt="movie.title" />
            <div class="card-body">
              <h2 class="card-title">{{ movie.title }}</h2>
              <p class="line-clamp-5 text-sm">{{ movie.overview }}</p>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </main>
</template>
