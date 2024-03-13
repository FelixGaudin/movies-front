<template>
  <section class="movie-list">
    <MovieDisplay v-for="film in films" :key="film.title"
                  :title="film.title" :year="film.year"
                  :affiche="film.poster" :genres="film.genres"
                  :languages="film.audio_streams" :colorMapping="colorMapping"/>
  </section>
</template>

<script>
import MovieDisplay from './MovieDisplay.vue';

export default {
  name: 'MovieList',
  components: {
    MovieDisplay
  },
  props: {
    films: Array,
    colorMapping: Object,
  }
}
</script>

<style scoped>
.movie-list {
    width: 90vw;
    margin: auto;
    margin-top: 30px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
}
</style>