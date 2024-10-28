<template>
  <div class="container">
    <HeaderView />
    <h3 style="margin-top: 15px">Directores</h3>
    
    <div class="card-grid">
      <div class="card" v-for="director in directors" :key="director.id">
        <NuxtLink :to="`/directors/${director.id}`">
          <div class="card-content">
            <img :src="director.imagen" :alt="director.nombre" class="director-image" />
            <h4>{{ director.nombre }}</h4>
          </div>
        </NuxtLink>
      </div>
    </div>
    
    <!-- Mensajes de estado -->
    <p v-if="error">Error: {{ error }}</p>
    <p v-else-if="!directors.length">Cargando datos...</p>
    
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const directors = ref([]);
const error = ref(null);

onMounted(async () => {
  try {
    const response = await fetch('/directors/directors.json');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    // Ordenar directores por nombre
    directors.value = data.sort((a, b) => a.nombre.localeCompare(b.nombre));
  } catch (err) {
    error.value = err.message;
    console.error('Error fetching directors:', err);
  }
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
.card {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
  overflow: hidden;
  padding: 10px;
  text-align: center;
}
.card:hover {
  transform: scale(1.05);
}
.card-content {
  padding: 10px;
}
.director-image {
  width: 100%; /* Ajusta el ancho de la imagen */
  height: 200px; /* Altura fija para todas las imágenes */
  object-fit: cover; /* Mantiene la proporción y recorta si es necesario */
  border-radius: 10px; /* Esquinas redondeadas para la imagen */
  margin-bottom: 10px; /* Espacio debajo de la imagen */
}
</style>





