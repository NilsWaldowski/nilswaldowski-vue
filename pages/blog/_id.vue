<template>
  <section class="container">
    <div>
      <h1 class="title">
        Blog Entry
      </h1>
    </div>
    <div>
      <h3>{{ title }}</h3>
      <p>{{ body }}</p>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  validate ({ params }) {
    return !isNaN(+params.id)
  },
  async asyncData ({ params, error }) {
    try {
      const { data } = await axios.get(`https://jsonplaceholder.typicode.com/posts/${+params.id}`)
      return data
    } catch (e) {
      error({ message: 'Post not found', statusCode: 404 })
    }
  }
}
</script>

<style>

</style>
