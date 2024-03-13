<template>
  <div id="app">
    <div>
      <!-- Search -->
			<b-field label="Nom du film">
            <b-input 
							v-model="query"
							@input="getFilteredMovies"
							>
						</b-input>
        </b-field>
      <b-field label="Genre du film">
            <b-taginput
                v-model="selectedGenres"
								:data="filteredGenres"
                icon="label"
                placeholder="Sélectionner un ou plusieurs genres"
                aria-close-label="Supprimer un genre"
								autocomplete
								open-on-focus
								@typing="getFilteredTags"
								@remove="getFilteredMovies"
								@add="getFilteredMovies"
								>
            </b-taginput>
        </b-field>
    </div>
		<!-- Display movies -->
    <MovieList :films="filteredMovies" />
  </div>
</template>

<script>
import MovieList from './components/MovieList.vue';
import data from './data/movies.json';

export default {
  name: 'App',
  components: {
    MovieList
  },
  data() {
    return {
      movies: data,
      filteredMovies: data,
      genres: [],
			selectedGenres: [],
			filteredGenres: [],
			query: "",
    }
  },
  methods: {
    getAllGenres() {
      let allGenres = new Set()
      this.filteredMovies.forEach(movie => {
          movie.genres.forEach(genre => {
            allGenres.add(genre)
          })
      });
      this.genres = Array.from(allGenres);
    },
		getFilteredMovies() {
			this.filteredMovies = this.movies.filter((movie) => {
				let isDisplay = true;
				// Genres filtering
				this.selectedGenres.forEach((genre) => {
					if (!movie.genres.includes(genre)) {
						isDisplay = false
					}
				})
				// Name filtering
				if (this.query != "") {
					if (movie.title
							.toString()
							.normalize("NFD")
							.replace(/[\u0300-\u036f]/g, "")
							.toLowerCase()
							.indexOf(
									this.query
											.toLowerCase()
											.normalize("NFD")
											.replace(/[\u0300-\u036f]/g, "")
									) < 0) {
							isDisplay = false
						}
				}
				return isDisplay
			})
			this.getAllGenres()
		},
    getFilteredTags(text) {
      this.filteredGenres = this.genres.filter((genre) => {
				return (!this.selectedGenres.includes(genre)) && genre
					.toString()
					.toLowerCase()
					.indexOf(text.toLowerCase()) >= 0
      })
			// this.getFilteredMovies()
    }
  },
  beforeMount() {
    this.getAllGenres()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
