<template>
  <section class="container">
    <div>
      <h1 class="title">
        About
      </h1>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {

    }
  },
  head () {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'My custom description'
        },
        {
          hid: 'title',
          name: 'title',
          content: 'My custom title'
        }
      ]
    }
  }
}
</script>

<style>

</style>
