<template>
  <PaginationBar @new-page="refreshGallery"/>
  <ImageGallery
      :images="images"
  />
</template>

<script>

import ImageGallery from "@/components/ImageGallery";
import PaginationBar from "@/components/PaginationBar";
export default {
  name: 'HomeView',
  props: {
    showAddTask: Boolean,
  },
  components: {
    PaginationBar,
    ImageGallery

  },
  data() {
    return {
      images: [],
    }
  },
  methods: {

    async refreshGallery(inputData) {
      const res = await fetch('https://picsum.photos/v2/list?page='+
          inputData.pageNumber+ '&limit=' +inputData.numberOfResults)
      const data = await res.json()
      //create display urls
      for (let i = 0; i < data.length; i++) {
        let s = data[i].download_url;
        s = s.substring(0, s.lastIndexOf("/"));
        data[i].display_url = s.substring(0, s.lastIndexOf("/")) + "/367/267"
      }
      this.images = data
    },

  },
  async created() {
    let defaultData = {pageNumber:1, numberOfResults:100}
    await this.refreshGallery(defaultData)
  },
}
</script>
