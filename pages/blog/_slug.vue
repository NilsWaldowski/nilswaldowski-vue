<template>
  <div>
    <header>
      <div>
        <img
          :src="post.fields.heroImage.fields.file.url + '?fit=scale&w=350&h=196'"
          :srcset="`${post.fields.heroImage.fields.file.url}?w=350&h=87&fit=fill 350w, ${post.fields.heroImage.fields.file.url}?w=1000&h=250&fit=fill 1000w, ${post.fields.heroImage.fields.file.url}?w=2000&h=500&fit=fill 2000w`"
          size="100vw"
          :alt="post.fields.heroImage.fields.description"
        >
      </div>
    </header>
    <section>
      <main>
        <div>
          <time>{{ ( new Date(post.fields.publishDate)).toDateString() }}</time>
          <h1>{{ post.fields.title }}</h1>
        </div>
        <div>
          <vue-markdown>{{post.fields.body}}</vue-markdown>
        </div>
      </main>
    </section>
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'
import {createClient} from '~/plugins/contentful.js'
const client = createClient()

export default {
  validate ({ params }) {
    return isNaN(+params.slug)
  },
  async asyncData ({ env, params, error }) {
    try {
      return client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        'fields.slug': params.slug
      }).then(entries => {
        // console.log(entries.items[0])
        return {
          post: entries.items[0]
        }
      })
    } catch (e) {
      error({ message: 'Post not found', statusCode: 404 })
    }
  },
  head () {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'SLUG description: ' + this.post.fields.title
        },
        {
          hid: 'title',
          name: 'title',
          content: 'SLUG title: ' + this.post.fields.title
        }
      ]
    }
  },
  components: {
    VueMarkdown
  }
}
</script>

<style scoped lang="scss">

</style>
