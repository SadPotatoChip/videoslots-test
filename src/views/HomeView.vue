<template>
  <PaginationBar ref="paginationBar"
      @update-per-page-number="updateResultNumber"
      @filter-by-author="filterByAuthor"
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

    async refreshGallery(pageNumber, numberOfResults, author = "") {

      const res = await fetch('https://picsum.photos/v2/list?page='+
          pageNumber + '&limit=' + numberOfResults)
      let data = await res.json()

      //filter by author
      //the way this works doesn't make much sense for the user considering they'd likely want to search the whole library of images
      //but I'm not really sure how I'd implement that since I couldn't find a way to ask the Picsum API for a list of images by an author
      if(author!=""){

        let filteredData = []
        for (let i = 0; i < data.length; i++) {
          if(data[i].author.toLowerCase().includes(author.toLowerCase())){

            filteredData.push(data[i])
          }
        }
        data = filteredData
      }else{
        this.$refs.paginationBar.$refs.authorsName.value = ""
      }

      //create display urls
      for (let i = 0; i < data.length; i++) {
        let s = data[i].download_url
        s = s.substring(0, s.lastIndexOf("/"))
        data[i].display_url = s.substring(0, s.lastIndexOf("/")) + "/367/267"
      }
      this.pageNumber = pageNumber
      this.numberOfResults = numberOfResults

      this.$refs.paginationBar.$refs.numberOfResults.innerHTML = "Page " + pageNumber

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
      //I couldn't figure out a way to fetch the total number of images on the site,
      //I thought about checking if anything is returned in refreshGallery() but it seemed like it would complicate things too much
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
    async filterByAuthor(author){
      await this.refreshGallery( parseInt(this.pageNumber), parseInt(this.numberOfResults), author)
    },

  },
  async created() {
    await this.refreshGallery(1,100)
  },
}
</script>
