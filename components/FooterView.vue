<template>
  <div class="footer">
    <button disabled class="footer-button">
      Copyright (c) {{ new Date().getFullYear() }} - Animated Movie Catalog
    </button>
  </div>
</template>

<style scoped>
.footer {
  background-color: #333;
  color: white;
  padding: 10px;
  text-align: center;
}
.footer-button {
  background-color: transparent;
  border: none;
  color: white;
}
</style>
