<template>
  <div>
    <div class="hero bg-grey-dark bg-cover shadow-in-big backdrop" :style="`background-image:url(${movie.images.backdrops.length !== 0 ? backdrop_host+movie.images.backdrops[0].file_path : '/ticket.jpg'});${movie.images.backdrops.length === 0 ? 'background-position:center;background-blend-mode:multiply;' : ''}`">
      <div class="container mx-auto flex items-end pb-8" style="height:350px;">
        <div class="w-1/4"></div>
        <div class="w-3/4 ml-12">
          <h1 class="font-heading text-white">{{ movie.title }}</h1>
          <div class="text-white mt-1">{{ movie.release_date.substr(0, 4) }} | &starf; {{ movie.vote_average }}</div>
        </div>
      </div>
    </div>
    <div class="container mx-auto flex">
      <div class="w-1/4 sm:w-1/3 -mt-20 px-5">
        <img :src="poster_host + movie.poster_path" alt="poster" class="poster">
      </div>
      <div class="w-3/4 sm:w-2/3 ml-12 mt-8">
        <p class="mb-8">{{ movie.overview }}</p>
        <div class="-mx-2">
          <h4 v-if="movie.images.backdrops.length > 1">Zdjęcia</h4>
          <div v-for="(image, id) in movie.images.backdrops" :key="id" v-if="id !== 0" class="sm:w-1/3 md:w-1/5 px-2 py-2 image-container" @click="openImageModal(image.file_path)">
            <img :src="poster_host + image.file_path" alt="clips" class="image-thumb">
          </div>
        </div>
        <div class="-mx-2">
          <h4 v-if="videos.results.length > 0">Wideo</h4>
          <div v-for="(video, id) in videos.results" :key="id" class="sm:w-1/3 md:w-1/5 px-2 py-2 video-container" @click="openVideoModal(video.key)">
            <img :src="`https://img.youtube.com/vi/${video.key}/mqdefault.jpg`" alt="videos" class="video-thumb">
          </div>
        </div>
      </div>
    </div>
    <Modal :show="showImageModal" @close="showImageModal = false">
      <img :src="'https://image.tmdb.org/t/p/w1280' + imagePath" alt="clips" class="modal-content">
    </Modal>
    <Modal :show="showVideoModal" @close="showVideoModal = false">
      <iframe width="750" height="422"  class="modal-content" :src="`https://www.youtube.com/embed/${videoKey}`" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </Modal>
  </div>
</template>

<script>
import axios from 'axios';
import Modal from '../../components/Modal';

export default {
  components: {
    Modal,
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
      backdrop_host: 'https://image.tmdb.org/t/p/w1280',
      showImageModal: false,
      showVideoModal: false,
      imagePath: '',
      videoKey: '',
    }
  },
  methods: {
    openImageModal(filePath) {
      this.imagePath = filePath;
      this.showImageModal = true;
    },
    openVideoModal(key) {
      this.videoKey = key;
      this.showVideoModal = true;
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
    background-position: center;
    width: 100vw;
    height: 350px;
  }
  .poster {
    position: relative;
    left: -20px;
  }
  .image-container, .video-container {
    display: inline-block;
  }
  .image-container img {
    cursor: pointer;
  }
  .video-container {
    position: relative;
  }
  .video-container:after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 90%;
    height: 80%;
    margin: 0.5rem;
    color: #fff;
    background: url('https://i.ya-webdesign.com/images/white-play-icon-png-3.png') no-repeat center / 35%;
    background-color: rgba(0,0,0,.5);
    cursor: pointer;
  }
  .modal-content {
    margin-bottom: -3px;
  }
</style>
