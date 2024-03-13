<template>
    <div class="card movie-card">
    <div class="card-image">
        <figure class="image">
        <img :src="affiche" alt="Placeholder image">
        </figure>
    </div>
    <div class="card-content">
        <div class="media">
            <div class="media-content">
                <p class="title is-4">{{ title }}</p>
                <p class="subtitle is-6">{{ year }}</p>
            </div>
        </div>

        <div class="content">
            <div>
                <!-- Genre -->
                <b-taglist class="centered">
                    <b-tag v-for="genre in genres" :key="genre"
                        type="is-info">{{ genre }}</b-tag>
                </b-taglist>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
export default {
  name: 'MovieDisplay',
  props: {
    title: String,
    year: String,
    affiche: String,
    genres: Array,
    languages: Array
  }
}
</script>

<style scoped>
.centered {
  display: flex;
  justify-content: center;
}

.movie-card {
    width: 20vw;
    margin: 5px;
    display: flex;
}

.card-content {
    flex: 1; /* Pour occuper l'espace disponible à gauche */
}

.card-image {
    width: 30%; /* Largeur de l'image */
}
</style>