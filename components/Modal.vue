<template>
  <div
    v-if="show"
    class="pin fixed flex items-center justify-center bg-transparent-black"
    @click.self="close"
  >
    <div class="relative bg-black shadow-lg max-w-2xl">
      <button v-if="showClose"
        aria-label="close"
        class="absolute top-0 right-0 text-xl text-gray-500 my-2 mx-4" 
        @click.prevent="close"
      >
        ×
      </button>
      <slot />
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Modal',
    props: {
      show: {
        required: true,
        type: Boolean
      },
      showClose: {
        type: Boolean,
        default: false,
      },
    },
    watch: {
      showing(value) {
        if (value) {
          return document.querySelector('body').classList.add('overflow-hidden');
        }

        document.querySelector('body').classList.remove('overflow-hidden');
      }
    },
    methods: {
      close() {
        this.$emit('close');
      }
    }
  }
</script>

<style scoped>
  .bg-transparent-black {
    background: rgba(0,0,0,.8);
  }
</style>