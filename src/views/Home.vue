<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">

    <GiphySearch />
  </div>
</template>

<script>
import GiphySearch from '@/components/GiphySearch.vue'

export default {
  name: 'Home',
  components: {
    GiphySearch
  }
}
</script>
