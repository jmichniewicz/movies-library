<template>
  <div class="container mx-auto py-16">
    <h1 class="font-heading uppercase mb-8">Polpular Movies</h1>
    <div class="movies-container flex flex-wrap -mx-4">
      <nuxt-link :to="'/movies/' + movie.id" v-for="movie in movies.results" :key="movie.id" class="w-full md:w-1/5 px-4 mb-12 md:mx-auto no-underline">
        <img :src="'https://image.tmdb.org/t/p/w185' + movie.poster_path" alt="poster">
        <div class="text-black font-semibold text-lg overflow-hidden whitespace-no-wrap overflow-dots pb-1">{{ movie.title }}</div>
        <div class="text-grey-darker text-base pb-1">{{ movie.release_date.substr(0, 4) }} | &starf; {{ movie.vote_average }}</div>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  components: {
    
  },
  asyncData ({ params, error }) {
    return axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=fa60ca0bb69cf9b8b1e987abf0e50d97&language=pl-PL&sort_by=popularity.desc&include_adult=false&include_video=false&page=1`)
    .then((res) => {
      return { movies: res.data }
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  },
  data() {
    return {
      
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
</style>
