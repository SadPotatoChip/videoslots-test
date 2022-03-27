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
      //create display urls
      for (let i = 0; i < data.length; i++) {
        let s = data[i].download_url;
        s = s.substring(0, s.lastIndexOf("/"));
        data[i].display_url = s.substring(0, s.lastIndexOf("/")) + "/367/267"
      }
      return data
    },

  },
  async created() {
    this.images = await this.fetchImages()
  },
}
</script>
