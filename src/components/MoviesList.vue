<script setup>
import MoviesCard from './MoviesCard.vue'

defineProps({
  movies: Array
})
</script>

<template>
  <main class="movies-container">
    <h2 class="movies-title">Список фильмов</h2>
    <slot />
    <div class="movies-list">
      <MoviesCard
        v-for="movie in movies"
        :key="movie.id"
        :title="movie.original_title"
        :raiting="movie.vote_average"
        :url_movie="movie.homepage ? movie.homepage : null"
      />
    </div>
  </main>
</template>

<style scoped>
.movies-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-width: 900px;
  margin: auto;
}

.movies-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, 200px);
  justify-content: center;
  padding: 0 20px;
  margin: 30px 0;
  gap: 30px 20px;
  width: 100%;
}

.movies-title {
  margin-top: 40px;
}

@media screen and (max-width: 900px) {
  .movies-list {
    justify-content: center;
  }
}
</style>
