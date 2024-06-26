<script setup>
defineProps({
  title: String,
  raiting: Number,
  url_movie: String
})
</script>

<template>
  <div :class="'card-container' + (url_movie ? '' : ' no-click')">
    <a class="card-link" :target="'_blank'" :href="url_movie">
      <div class="card-info">
        <div class="card-image-container">
          <div class="card-rating">
            <span class="card-rating__value">{{ raiting }}</span>
          </div>
          <img class="card-image" src="https://pixy.org/src/16/167145.png" alt="" />
          <div :class="'card-link__checked ' + (url_movie ? '' : 'card-link__checked_no-link')">
            <span class="card-link__check">{{
              url_movie ? 'Перейти к фильму' : 'Нет ссылки'
            }}</span>
          </div>
        </div>

        <h3 class="card-title">{{ title }}</h3>
      </div>
    </a>
  </div>
</template>

<style scoped>
.card-container {
  position: relative;
  border-radius: 2%;
  background-color: rgb(119, 119, 119);
}

.card-container:hover {
  cursor: pointer;
  box-shadow: inset 0px 0 40px 5px rgba(0, 0, 0, 0.404);
}

.card-info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}

.card-link {
  text-decoration: none;
  color: inherit;
}

.card-link__checked {
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 10px;
  right: 4px;
  border-radius: 6%;
  background-color: rgb(0, 126, 21);
}

.card-link__checked_no-link {
  background-color: rgb(126, 0, 0);
}

.card-link__check {
  font-size: 12px;
  text-align: center;
}

.no-click:hover {
  cursor: not-allowed;
  box-shadow: none;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-image-container {
  position: relative;
  margin: 0;
  padding: 0;
}

.card-rating {
  position: absolute;
  left: 5px;
  top: 5px;
  border-radius: 50%;
  width: 25px;
  height: 25px;
  text-align: center;
  background-color: rgb(119, 119, 119);
}

.card-rating__value {
  text-align: center;
  font-size: 12px;
}

.card-title {
  margin: 0;
  text-align: center;
  padding: 5px;
}
</style>
