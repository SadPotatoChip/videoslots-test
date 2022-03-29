<template>
  <header>
    <form @submit="onSubmit" class="pagination-form">
      <div class="form-control">
        <label>Results Per Page</label>
        <input type="text" v-model="numberOfResults" name="numberOfResults" placeholder="100" />
      </div>
      <div class="form-control">
        <label>Page</label>
        <input type="text" v-model="pageNumber" name="pageNumber" placeholder="1" />
      </div>
      <input type="submit" value="Filter" class="btn btn-block" />
    </form>
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
    onSubmit(e) {
      e.preventDefault()

      const newPage = {
        pageNumber: this.pageNumber,
        numberOfResults: this.numberOfResults,
      }
      this.$emit('new-page', newPage)

    },
  },
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: start;
  align-items: start;
  margin-bottom: 20px;
}
.pagination-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

</style>
