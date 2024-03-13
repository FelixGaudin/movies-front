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
                <h4>Genres</h4>
                <b-taglist>
                    <b-tag v-for="genre in genres" :key="genre"
                        type="is-info">{{ genre }}</b-tag>
                </b-taglist>
            </div>
            <br>
            <div>
                <h4>languages</h4>
                <b-taglist>
                    <b-tag v-for="lang in languages" :key="lang"
                        type="is-warn">{{ lang }}</b-tag>
                </b-taglist>
                <!-- Genre -->
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
.movie-card {
    width: 15vw;
    margin: 5px;
}
</style>