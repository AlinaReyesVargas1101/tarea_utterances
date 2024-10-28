<template>
  <div class="container">
    <HeaderView />
    <div v-if="loading">Cargando...</div>
    <div v-else-if="error">{{ error }}</div>
    <div v-else-if="studio" class="studio-info">
      <h2>{{ studio.nombre }}</h2>
      <img :src="studio.logo" :alt="studio.nombre" class="studio-logo">
      <p><strong>Fundado:</strong> {{ new Date(studio.fundacion).toLocaleDateString() }}</p>
      <p><strong>Fundadores:</strong> {{ studio.fundador.join(', ') }}</p>
      <p><strong>Sede:</strong> {{ studio.sede }}</p>
      <p><a :href="studio.link" target="_blank" class="website-link">Sitio Web Oficial</a></p>
      
      <h3>Películas Producidas:</h3>
      <ul v-if="movies.length > 0" class="movie-list">
        <li v-for="movie in movies" :key="movie.id" class="movie-item">
          <NuxtLink :to="`/movies/${movie.id}`" class="movie-link">
            {{ movie.titulo }} ({{ movie.año }})
          </NuxtLink>
        </li>
      </ul>
      <p v-else>No se encontraron películas para este estudio.</p>
    </div>
    <div v-else>
      <p>Estudio no encontrado.</p>
    </div>
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const studio = ref(null);
const movies = ref([]);
const loading = ref(true);
const error = ref(null);
const route = useRoute();

onMounted(async () => {
  try {
    const studioResponse = await fetch('/studios/studios.json');
    if (!studioResponse.ok) {
      throw new Error('Error al cargar los detalles del estudio');
    }
    const studiosData = await studioResponse.json();

    const id = parseInt(route.params.id);
    studio.value = studiosData.find(s => s.id === id);

    if (studio.value) {
      const moviesResponse = await fetch('/movies/movies.json');
      if (!moviesResponse.ok) {
        throw new Error('Error al cargar las películas');
      }
      const moviesData = await moviesResponse.json();
      movies.value = moviesData.filter(m => m.estudio === id);
    } else {
      throw new Error('Estudio no encontrado');
    }
  } catch (err) {
    console.error('Error:', err);
    error.value = err.message || 'Ocurrió un error al cargar los datos.';
  } finally {
    loading.value = false;
  }
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.studio-info {
  background-color: #f9f9f9;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
}

.studio-logo {
  max-width: 200px;
  height: auto;
  margin-bottom: 15px;
}

.website-link {
  display: inline-block;
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #0077cc;
  color: white;
  text-decoration: none;
  border-radius: 4px;
}

.movie-list {
  list-style-type: none;
  padding: 0;
}

.movie-item {
  margin-bottom: 10px;
}

.movie-link {
  text-decoration: none;
  color: #0077cc;
}

.movie-link:hover {
  text-decoration: underline;
}
</style>




