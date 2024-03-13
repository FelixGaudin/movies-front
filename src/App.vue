<template>
  <div id="app">
    <div class="search">
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
    <MovieList :films="filteredMovies" :colorMapping="colorMapping" />
  </div>
</template>

<script>
import MovieList from './components/MovieList.vue';
import data from './data/movies.json';

let sortedMovies = data.slice().sort((a, b) => {
	return a.title.localeCompare(b.title);
}); 

export default {
  name: 'App',
  components: {
    MovieList
  },
  data() {
    return {
      movies: sortedMovies,
      filteredMovies: sortedMovies,
      genres: [],
			selectedGenres: [],
			filteredGenres: [],
			query: "",
			colorMapping: {},
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
			// Generate color mapping

			const colors = [
					"#FF5733", "#FFD700", "#FF6347", "#7FFF00", "#20B2AA", "#800080", "#FF4500",
					"#008080", "#FF1493", "#800000", "#808000", "#FFA500", "#8A2BE2", "#00FF00",
					"#000080", "#00FFFF", "#4B0082", "#A52A2A", "#8B0000", "#800080", "#FF69B4",
					"#1E90FF", "#ADFF2F", "#FF7F50", "#DC143C", "#228B22", "#4682B4", "#9400D3",
					"#00BFFF", "#6A5ACD", "#FFA07A", "#D2691E", "#008B8B", "#FF8C00", "#8B008B",
					"#556B2F", "#9932CC", "#E9967A", "#9400D3", "#FA8072", "#9370DB"
			];
			this.genres.forEach((genre, idx) => {
					this.colorMapping[genre] = colors[idx % colors.length];
			});
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
.search {
	margin: auto;
	align-self: center;
	width: 50vw;
}
</style>
