<template>
  <header>
    <div >
      <label>Results Per Page: </label>
      <input type="text" @change='$emit("update-per-page-number", $event.target.value)' name="numberOfResults" placeholder="100" />
    </div>
    <div >
      <input type="button" @click='$emit("previous-page", $event.target.value)' value="Previous Page"/>
    </div>
    <label id="pageLabel">Page 1</label>
    <div>
      <input type="button" @click='$emit("next-page", $event.target.value)' value="Next Page"/>
    </div>
  </header>
</template>

<script>

export default {
  name: 'PaginationBar',
  data() {
    return {
      pageNumber: 1,
      numberOfResults: 100,
    }
  },
  methods: {
  },
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  margin-bottom: 20px;
}

</style>
