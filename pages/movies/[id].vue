<template>
  <div class="container">
    <HeaderView />
    <div v-if="movie">
      <img :src="movie.image" alt="Movie poster" />
      <h1>{{ movie.titulo }}</h1>
      <p><strong>Año:</strong> {{ movie.año }}</p>
      <p><strong>Estreno:</strong> {{ movie.estreno }}</p>
      <p><strong>Géneros:</strong> {{ movie.genero.join(', ') }}</p>
      <p><strong>Duración:</strong> {{ movie.duracion }} minutos</p>
      <p><strong>Idioma:</strong> {{ movie.idioma }}</p>
      <p><strong>Clasificación:</strong> {{ movie.clasificacion }}</p>
      <p><strong>Estudio:</strong> 
        <router-link :to="`/studios/${movie.estudio}`">
          {{ getStudioName(movie.estudio) }}
        </router-link>
      </p>
      
      <p><strong>Director:</strong>
        <span v-for="(directorId, index) in movie.direccion" :key="directorId">
          <router-link :to="`/directors/${directorId}`">
            {{ getDirectorName(directorId) }}
          </router-link>
          <span v-if="index < movie.direccion.length - 1">, </span>
        </span>
      </p>
    </div>
    <div v-else>
      <p>Cargando película o no se encontró la información...</p>
    </div>
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const movie = ref(null);
const directors = ref([]);
const studios = ref([]);
const route = useRoute();

// Función para buscar el nombre del director usando su ID
const getDirectorName = (directorId) => {
  const director = directors.value.find(d => d.id === directorId);
  return director ? director.nombre : 'Desconocido';
};

// Función para buscar el nombre del estudio usando su ID
const getStudioName = (studioId) => {
  const studio = studios.value.find(s => s.id === studioId);
  return studio ? studio.nombre : 'Desconocido';
};

onMounted(async () => {
  try {
    // Cargar películas
    const movieResponse = await fetch('/movies/movies.json');
    if (!movieResponse.ok) {
      throw new Error('Error al cargar las películas');
    }
    const movies = await movieResponse.json();
    const id = parseInt(route.params.id);
    movie.value = movies.find(m => m.id === id);

    // Cargar directores
    const directorResponse = await fetch('/directors/directors.json');
    if (!directorResponse.ok) {
      throw new Error('Error al cargar los directores');
    }
    directors.value = await directorResponse.json();

    // Cargar estudios
    const studioResponse = await fetch('/studios/studios.json');
    if (!studioResponse.ok) {
      throw new Error('Error al cargar los estudios');
    }
    studios.value = await studioResponse.json();
  } catch (err) {
    console.error('Error al cargar los datos:', err);
  }
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

p {
  margin-bottom: 10px;
}

strong {
  font-weight: bold;
}

a {
  color: #0077cc;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  padding-Top: 25px;
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>