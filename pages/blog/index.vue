<template>
  <section>
    <div>
      <h1>
        Blog
      </h1>
      <ul>
        <li v-for="(post, index) in posts" class="posts" :key="index">
          <article-preview :post="post"></article-preview>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import ArticlePreview from '~/components/article-preview.vue'
import {createClient} from '~/plugins/contentful.js'
// import { TimelineMax, Sine } from 'gsap'

const client = createClient()

export default {
  /*
  JS GSAP Transition
  transition: {
    mode: 'out-in',
    css: false,

    enter (el, done) {
      let tl = new TimelineMax({ onComplete: done })

      tl.add('start')

      tl.from(el, 0.8, {
        scale: 0.95,
        transformOrigin: '50% 50%',
        ease: Sine.easeOut
      }, 'start')

      tl.staggerFrom('.posts', 0.4, {
        ease: Sine.easeOut,
        opacity: 0,
        y: '150px'
      }, 0.1, 0.2)
    }
  },
  */

  /*
  CSS Transition
  transition (to, from) {
    // console.log(to)
    // console.log('from ', from)
    if (!from) return 'slide-left'
    return +to.query.page < +from.query.page ? 'slide-right' : 'slide-left'
  },
  */
  // transition: 'fadeOpacity',
  // `env` is available in the context object

  /*
  CSS Transition
  transition: {
    mode: 'out-in',
    css: false,
    beforeEnter (el) {
      console.log(el)
      TweenMax.set(el, {
        transformPerspective: 600,
        perspective: 300,
        transformStyle: 'preserve-3d'
      })
    },
    enter (el, done) {
      TweenMax.to(el, 1, {
        rotationY: 360,
        transformOrigin: '50% 50%',
        ease: Back.easeOut
      })
      done()
    },
    leave (el, done) {
      TweenMax.to(el, 1, {
        rotationY: 0,
        transformOrigin: '50% 50%',
        ease: Back.easeIn
      })
      done()
    }
  },
  */
  data () {
    return {}
  },
  asyncData ({env}) {
    return Promise.all([
      // fetch the owner of the blog
      client.getEntries({
        'sys.id': env.CTF_PERSON_ID
      }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ]).then(([entries, posts]) => {
      // return data that should be available
      // in the template
      return {
        person: entries.items[0],
        posts: posts.items
      }
    }).catch(console.error)
  },
  head () {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'BLOG description'
        },
        {
          hid: 'title',
          name: 'title',
          content: 'BLOG title'
        }
      ]
    }
  },
  components: {
    ArticlePreview
  }
}
</script>

<style scoped lang="scss">
.fadeOpacity-enter-active, .fadeOpacity-leave-active {
  transition: opacity .35s ease-out;
}

.fadeOpacity-enter, .fadeOpacity-leave-active {
  opacity: 0;
}
</style>
