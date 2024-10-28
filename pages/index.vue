<template>
  <div class="container">
    <HeaderView />
    <div class="content">
      <h2 class="title">Welcome to the Animated Movie Catalog</h2>
      <p class="description">
        Explore our collection of animated movies, directors, and studios.
      </p>
      <ul class="links">
        <li><NuxtLink to="/movies" class="link-item">Browse Movies</NuxtLink></li>
        <li><NuxtLink to="/directors" class="link-item">Explore Directors</NuxtLink></li>
        <li><NuxtLink to="/studios" class="link-item">Discover Studios</NuxtLink></li>
      </ul>
    </div>
    <FooterView />
  </div>
</template>

<style scoped>
.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  background-color: #f8f9fa; /* Light background */
  border-radius: 8px; /* Rounded corners */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
}

.content {
  margin-bottom: 40px;
  padding: 20px;
  background-color: #ffffff; /* White background for content */
  border-radius: 8px; /* Rounded corners */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Subtle shadow */
}

.title {
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 10px; /* Space below title */
}

.description {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 20px;
}

.links {
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

.links li {
  margin-bottom: 15px; /* Increased space between links */
}

.link-item {
  color: #0077cc;
  text-decoration: none;
  font-size: 1.1rem;
  padding: 10px 15px; /* Padding for a button-like feel */
  border-radius: 5px; /* Rounded corners */
  transition: background-color 0.3s ease; /* Smooth transition */
}

.link-item:hover {
  background-color: #e6f7ff; /* Light blue background on hover */
  text-decoration: underline;
  color: #005ea6;
}
</style>

