<script setup>
import axios from 'axios'
import { onMounted, ref, watch } from 'vue'
import Header from './components/Header.vue'
import MoviesList from './components/MoviesList.vue'

const MOVIES_API = 'http://185.185.69.80:8082'
const movies = ref([])
const searchMovie = ref('')
const pagination_page = ref(0)
const isLoading = ref(false)
const allMovies = ref(false)

const pagination_plus = () => {
  if (pagination_page.value >= 0) {
    pagination_page.value++
    allMovies.value = false
  }
  return
}

const pagination_minus = () => {
  if (pagination_page.value > 0) {
    pagination_page.value--
    allMovies.value = false
  }
  return
}

const isAllMovies = () => {
  if (!allMovies.value) {
    allMovies.value = true
  }
  return
}

onMounted(async () => {
  try {
    isLoading.value = true
    await axios
      .post(`${MOVIES_API}/list`, {
        page: Number(pagination_page.value),
        page_size: Number(16)
      })
      .then((response) => {
        const { data } = response.data
        movies.value = data
      })
      .finally(() => (isLoading.value = false))
  } catch (error) {
    console.log(error)
  }
})

watch(searchMovie, async () => {
  try {
    isLoading.value = true
    await axios
      .post(`${MOVIES_API}/list`, {
        page_size: Number(16),
        search: String(searchMovie.value)
      })
      .then((response) => {
        const { data } = response.data
        movies.value = data
      })
      .finally(() => (isLoading.value = false))
  } catch (error) {
    console.log(error)
  }
})

watch(pagination_page, async () => {
  try {
    isLoading.value = true
    await axios
      .post(`${MOVIES_API}/list`, {
        page: Number(pagination_page.value),
        page_size: Number(16),
        search: String(searchMovie.value)
      })
      .then((response) => {
        const { data } = response.data
        movies.value = data
      })
      .finally(() => (isLoading.value = false))
  } catch (error) {
    console.log(error)
  }
})

watch(allMovies, async () => {
  try {
    isLoading.value = true
    await axios
      .post(`${MOVIES_API}/list`, {
        search: String(searchMovie.value)
      })
      .then((response) => {
        const { data } = response.data
        movies.value = data
      })
      .finally(() => (isLoading.value = false))
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <Header />
  <MoviesList :movies="movies">
    <div class="search-container">
      <input class="search-input" type="text" placeholder="Поиск..." v-model="searchMovie" />
    </div>
    <div class="movies-pagination">
      <button
        v-bind:disabled="isLoading || pagination_page === 0"
        @click="pagination_minus"
        class="movies-pagination__btn"
      >
        Предыдущая стр
      </button>
      <button v-bind:disabled="isLoading" @click="pagination_plus" class="movies-pagination__btn">
        Следующая стр
      </button>
      <button
        v-bind:disabled="isLoading || allMovies"
        @click="isAllMovies"
        class="movies-pagination__btn"
      >
        Все фильмы
      </button>
    </div>
  </MoviesList>
</template>

<style scoped>
.search-container {
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
  margin-top: 30px;
  padding: 0 15px;
}

.search-input {
  width: 100%;
  max-width: 300px;
  height: 20px;
  border: 1px solid black;
  border-radius: 5px;
  padding: 0 10px;
  text-align: center;
}

.search-input:focus {
  outline: none;
  text-align: center;
}

.search-input::placeholder {
  text-align: center;
}

.movies-pagination {
  display: flex;
  justify-content: right;
  margin-top: 30px;
  width: 100%;
  padding-right: 20px;
}

.movies-pagination__btn {
  height: 30px;
  margin-right: 5px;
  padding: 5px 10px;
  border-radius: 5px;
  border: none;
  background-color: rgb(119, 119, 119);
  color: white;
  cursor: pointer;
}

.movies-pagination__btn:active {
  background-color: rgba(119, 119, 119, 0.26);
  padding-bottom: 3px;
}

.movies-pagination__btn:hover {
  background-color: rgba(119, 119, 119, 0.486);
}

.movies-pagination__btn:disabled {
  background-color: rgba(119, 119, 119, 0.034);
  cursor: not-allowed;
  padding: 5px 10px;
}

.movies-pagination__btn:last-child {
  margin-right: 0;
}

@media screen and (max-width: 900px) {
  .movies-pagination {
    display: flex;
    justify-content: center;
    padding-right: 0;
  }
}
</style>
