<template>
  <div class="header">
    <div class="nav">
      <NuxtLink class="nav-link" to="/">Home</NuxtLink>
      <NuxtLink class="nav-link" to="/movies">Movies</NuxtLink>
      <NuxtLink class="nav-link" to="/directors">Directors</NuxtLink>
      <NuxtLink class="nav-link" to="/studios">Studios</NuxtLink>
    </div>
  </div>
</template>

<style scoped>
.header {
  background-color: #333;
  color: white;
  padding: 20px 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.nav {
  display: flex;
  justify-content: center;
  gap: 30px;
}
.nav-link {
  color: white;
  text-decoration: none;
  padding: 10px 20px;
  font-size: 1.1rem;
  transition: background-color 0.3s ease;
}
.nav-link:hover {
  background-color: #555;
  border-radius: 5px;
}
</style>

