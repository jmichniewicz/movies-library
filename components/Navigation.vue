<template>
  <div class="bg-blue-darkest font-semibold">
    <div class="container mx-auto flex justify-between items-center py-6">
      <nuxt-link to="/" class="text-white no-underline hover:text-grey-light">
        <img style="margin:-15px 0;width:50px;height:auto;" src="https://www.themoviedb.org/assets/1/v4/logos/primary-green-d70eebe18a5eb5b166d5c1ef0796715b8d1a2cbc698f96d311d62f894ae87085.svg" alt="logo">
      </nuxt-link>
      <div class="flex">
        <nuxt-link to="/" class="text-white no-underline hover:text-grey-light ml-16">Popular</nuxt-link>
        <nuxt-link to="/new" class="text-white no-underline hover:text-grey-light ml-16">New</nuxt-link>
        <nuxt-link to="/pl" class="text-white no-underline hover:text-grey-light ml-16">Polish</nuxt-link>
        <nuxt-link to="/login" class="text-white no-underline hover:text-grey-light ml-16">LogIn</nuxt-link>
      </div>
    </div>
  </div>
</template>
