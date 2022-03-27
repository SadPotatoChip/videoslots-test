<template>
  <ImageGallery
      :images="images"
  />
</template>

<script>

import ImageGallery from "@/components/ImageGallery";
export default {
  name: 'HomeView',
  props: {
    showAddTask: Boolean,
  },
  components: {
    ImageGallery

  },
  data() {
    return {
      images: [],
    }
  },
  methods: {

    async fetchImages() {
      const res = await fetch('https://picsum.photos/v2/list')

      const data = await res.json()
      for (let i = 0; i < data.length; i++) {
        data[i].display_url = data[i].download_url.substring(0, data[i].download_url.length-10) + "/367/267"
      }
      return data
    },

  },
  async created() {
    this.images = await this.fetchImages()
  },
}
</script>
