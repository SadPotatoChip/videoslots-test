<template>
  <div :key="image.id" v-for="image in images">
    <DisplayableImage
        :image="image"
    />
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

</style>
