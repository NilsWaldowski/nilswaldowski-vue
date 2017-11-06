<template>
  <section class="container">
    <div>
      <h1 class="title">
        Blog
      </h1>
      <ul class="posts">
        <li v-for="post in posts">
          <nuxt-link :to="'/blog/'+post.id">{{ post.title }}</nuxt-link>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData () {
    const { data } = await axios.get('https://jsonplaceholder.typicode.com/posts')
    return { posts: data }
  }
}
</script>

<style scoped lang="scss">

</style>
