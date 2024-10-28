<template>
  <div class="container">
    <HeaderView />
    <h3 style="margin-top: 15px">Películas Animadas</h3>
    <p>Lista de películas animadas populares de los últimos años.</p>
    <div class="card-grid">
      <div class="card" v-for="movie in movies" :key="movie.id">
        <NuxtLink :to="`/movies/${movie.id}`">
          <img :src="movie.image" alt="Movie poster" />
          <div class="card-content">
            <h3>{{ movie.title }}</h3>
            <p>{{ movie.description }}</p>
          </div>
        </NuxtLink>
      </div>
    </div>
    <p v-if="!movies.length">Cargando o no hay datos disponibles...</p>
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const movies = ref([]);
const error = ref(null);

onMounted(async () => {
  try {
    //const response = await fetch('/movies/movies.json');
    const response = await fetch('http://cms-una.unaux.com/:animation-movies-4/api/content/items/Movies');
    console.log(response)
    if (!response.ok) {
      throw new Error('Error fetching movies');
    }
    movies.value = await response.json();
  } catch (err) {
    error.value = err;
    console.error('Error fetching movies:', err);
  }
});
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
.card {
  background-color: white;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}
.card:hover {
  transform: scale(1.05);
}
.card img {
  width: 100%;
  height: 300px;
  object-fit: cover;
}
.card-content {
  padding: 10px;
}
</style>









