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
                        :style="getStyle(genre)"
                        >{{ genre }}</b-tag>
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
    languages: Array,
    colorMapping: Object,
  },
  methods: {
    getStyle(genre) {
        return { 
            backgroundColor: this.colorMapping[genre], 
            color: this.textColor(this.colorMapping[genre]),
        }
    },
    textColor(colorHex) {
        const r = parseInt(colorHex.substr(1, 2), 16);
        const g = parseInt(colorHex.substr(3, 2), 16);
        const b = parseInt(colorHex.substr(5, 2), 16);
        const luminosity = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
        return luminosity > 0.5 ? '#000000' : '#FFFFFF';
    }
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