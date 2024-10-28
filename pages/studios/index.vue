<template>
  <div class="container">
    <HeaderView />
    <h3 style="margin-top: 15px">Animation Studios</h3>
    <p>Lista de estudios de animación destacados.</p>
    
    <div class="card-grid">
      <div class="card" v-for="studio in studios" :key="studio.id">
        <NuxtLink :to="`/studios/${studio.id}`">
          <img :src="studio.logo" alt="Logo del estudio" class="studio-logo" />
          <div class="card-content">
            <h4>{{ studio.nombre }}</h4>
          </div>
        </NuxtLink>
      </div>
    </div>

    <p v-if="!studios.length">Cargando o no hay datos disponibles...</p>
    <FooterView />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const studios = ref([]);
const error = ref(null);

onMounted(async () => {
  try {
    const response = await fetch('/studios/studios.json');
    if (!response.ok) {
      throw new Error('Error fetching studios');
    }
    studios.value = await response.json();
  } catch (err) {
    error.value = err;
    console.error('Error fetching studios:', err);
  }
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Adaptable según el espacio disponible */
  gap: 20px;
}

.card {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
  overflow: hidden;
  text-align: center;
  padding: 10px;
}

.card:hover {
  transform: scale(1.05);
}

.studio-logo {
  width: 50px;
  height: 50px;
  margin-bottom: 10px;
}
</style>
