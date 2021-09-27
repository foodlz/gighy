<template>
  <div class="gif" :style="`background-image: url('${gif.images.fixed_width.url}')`">
  </div>
</template>

<script>
export default {
  name: "Gif",
  props: ['gif'],
}
</script>

<style scoped>
.gif {
  width: 200px;
  height: 200px;
  margin-bottom: 5px;
  margin-left: 5px;
  display: inline-flex;
  justify-content: center;
  align-items: flex-end;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
</style>