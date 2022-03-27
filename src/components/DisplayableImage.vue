<template>
  <div>
    <a :href="image.download_url" target="_blank">
      <img :src="image.display_url">
    </a>
    <p class="author">Author: {{ image.author }}</p>
    <p class="imageId">ID: {{ image.id }}</p>
  </div>
</template>

<script>
export default {
  name: 'DisplayableImage',
  props: {
    image: Object,
  },
}
</script>

<style scope>
  .author {
    text-overflow: ellipsis
  }
  .imageId {
    text-overflow: ellipsis
  }
</style>
