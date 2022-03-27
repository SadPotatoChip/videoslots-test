<template>
  <div class="flex-container">
    <div :key="image.id" v-for="image in images">
      <DisplayableImage
          :image="image"
      />
    </div>
  </div>
</template>

<script>
import DisplayableImage from './DisplayableImage'

export default {
  name: 'ImageGallery',
  props: {
    images: Array,
  },
  components: {
    DisplayableImage,
  },
}
</script>

<style scoped>
  .flex-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
</style>
