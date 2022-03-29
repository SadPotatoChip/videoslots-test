<template>
  <PaginationBar
      @update-per-page-number="updateResultNumber"
      @update-page-number="updatePageNumber"
      @next-page="nextPage"
      @previous-page="previousPage"

  />
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

    async refreshGallery(pageNumber, numberOfResults) {

      const res = await fetch('https://picsum.photos/v2/list?page='+
          pageNumber + '&limit=' + numberOfResults)
      const data = await res.json()
      console.log(pageNumber + "  " + numberOfResults)
      //create display urls
      for (let i = 0; i < data.length; i++) {
        let s = data[i].download_url;
        s = s.substring(0, s.lastIndexOf("/"));
        data[i].display_url = s.substring(0, s.lastIndexOf("/")) + "/367/267"
      }
      this.pageNumber = pageNumber
      this.numberOfResults = numberOfResults
      document.getElementById("pageLabel").innerHTML = "Page " + pageNumber


      this.images = data
    },
    async updateResultNumber(numberOfResults = 100){
      await this.refreshGallery(this.pageNumber, numberOfResults)
    },
    async updatePageNumber(pageNumber = 1){
      await this.refreshGallery(pageNumber, this.numberOfResults)
    },
    async nextPage(){
      let currentPage = parseInt(this.pageNumber)
      if(currentPage * this.numberOfResults < 1000){
        await this.updatePageNumber( currentPage + 1)
      }
    },
    async previousPage(){
      let currentPage = parseInt(this.pageNumber)
      if(currentPage > 1){
        await this.updatePageNumber( currentPage - 1)
      }
    },


  },
  async created() {
    await this.refreshGallery(1,100)
  },
}
</script>
