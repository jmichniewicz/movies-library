<template>
  <div>
  <div class="hero bg-grey-dark bg-cover shadow-in-big" :style="'background-image:url('+backdrop_host + movie.images.backdrops[0].file_path+')'">
    <div class="container mx-auto flex items-end pb-8" style="height:350px;">
      <div class="w-1/4"></div>
      <div class="w-3/4 ml-12">
        <h1 class="font-heading text-white">{{ movie.title }}</h1>
        <div class="text-white mt-1">{{ movie.release_date.substr(0, 4) }} | &starf; {{ movie.vote_average }}</div>
      </div>
    </div>
  </div>
  <div class="container mx-auto flex">
    <div class="w-1/4 -mt-20 px-5">
      <img :src="poster_host + movie.poster_path" alt="poster">
    </div>
    <div class="w-3/4 ml-12 mt-8">
      <p class="mb-8">{{ movie.overview }}</p>
      <div class="-mx-2">
        <img v-for="(image, id) in movie.images.backdrops" :key="id" :src="poster_host + image.file_path" alt="clips" class="w-1/6 px-2 py-2">
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  components: {
    
  },
  asyncData ({ params, error }) {
    return axios.get(`https://api.themoviedb.org/3/movie/${params.id}?api_key=fa60ca0bb69cf9b8b1e987abf0e50d97&language=pl-PL&append_to_response=images&include_image_language=null`)
    .then((res) => {
      return axios.get(`https://api.themoviedb.org/3/movie/${params.id}/videos?api_key=fa60ca0bb69cf9b8b1e987abf0e50d97`)
      .then((res2) => {
        return { 
          movie: res.data,
          videos: res2.data
        }
      })
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  },
  data() {
    return {
      poster_host: 'https://image.tmdb.org/t/p/w300',
      backdrop_host: 'https://image.tmdb.org/t/p/w1280'
    }
  },
  head() {
    return {
      title: this.movie.title + ' | Movies Library'
    }
  }
}
</script>

<style scoped>
  .backdrop {
    background-size: cover !important;
    width: 100vw;
    height: 350px;
  }
</style>
