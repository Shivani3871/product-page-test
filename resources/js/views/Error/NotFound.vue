<template>
    <div class="not-found">
      <h1 class="not-found__title">404</h1>
      <p class="not-found__message">Sorry, the page you are looking for does not exist.</p>
      <router-link :to="{ name: 'product', params: { 'slug': slug }}" class="not-found__link">Go to Home</router-link>
    </div>
  </template>
  
  <script>
  export default {
    name: 'not-found',
    data() {
        return {
        slug: 'fall-limited-edition-sneakers'
        };
    },
  };
  </script>