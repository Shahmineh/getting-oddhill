<template>
  <!-- Since vue-markdown -->
  <div>
    <div class="field is-grouped is-grouped-centered">
      <p class="control">
        <router-link
          class="button is-primary"
          :to="`/post/edit/${$route.params.id}`">Edit post</router-link>
      </p>
      <p class="control">
        <a
          class="button is-danger"
          @click="deletePost($route.params.id)">Delete post</a>
      </p>
    </div>
    <vue-markdown
      :source="post.message"
      class="content"/>

  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'

export default {
  components: {
    VueMarkdown
  },
  props: ['post'],
  methods: {
    deletePost (id) {
      this.$store.dispatch('deletePost', { id })
      this.$router.push(`/`)
    }
  }
}

</script>

<style scoped lang="scss">

</style>
