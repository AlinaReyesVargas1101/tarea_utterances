<template>
  <div class="container">
    <HeaderView />
    <div v-if="director">
      <h2>Director: {{ director.nombre }}</h2>
      <img :src="director.imagen" :alt="director.nombre" class="u-max-full-width" style="width: 30%;">

      <p>Born: {{ formatDate(director.nacimiento) }}</p>
      <p v-if="director.defuncion">Died: {{ formatDate(director.defuncion) }}</p>
      <p>Nationality: {{ director.nacionalidad }}</p>
      <p>Education: {{ director.educacion }}</p>
      <p><a :href="director.link" target="_blank">More Info</a></p>

      <h3>Movies Directed:</h3>
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <NuxtLink :to="`/movies/${movie.id}`">{{ movie.titulo }}</NuxtLink>
        </li>
      </ul>
    </div>
    <p v-else-if="error">Error: {{ error }}</p>
    <p v-else>Cargando datos...</p>
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const director = ref(null);
const movies = ref([]);
const error = ref(null);
const route = useRoute();

const formatDate = (dateString) => {
  if (!dateString) return 'N/A';
  const date = new Date(dateString);
  return date.toLocaleDateString('es-ES', { year: 'numeric', month: 'long', day: 'numeric' });
};

onMounted(async () => {
  const directorId = route.params.id;

  try {
    // Fetch director details
    const directorResponse = await fetch(`/directors/directors.json`);
    if (!directorResponse.ok) {
      throw new Error(`HTTP error! status: ${directorResponse.status}`);
    }
    const directorsData = await directorResponse.json();
    director.value = directorsData.find(d => d.id === parseInt(directorId));

    if (!director.value) {
      throw new Error('Director not found');
    }

    // Fetch movies directed by this director
    const moviesResponse = await fetch(`/movies/movies.json`);
    if (!moviesResponse.ok) {
      throw new Error(`HTTP error! status: ${moviesResponse.status}`);
    }
    const moviesData = await moviesResponse.json();
   movies.value = moviesData.filter(movie => movie.direccion.includes(director.value.id));
  } catch (err) {
    error.value = err.message;
    console.error('Error fetching director or movies:', err);
  }
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}

img {
  margin-right: 15px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}
</style>
