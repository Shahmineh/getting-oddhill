<template>
  <div class="posts">
    <div
      class="content"
      ref="postsstage"
      id="posts-stage">

      <section class="section">
        <div class="container">
          <div class="level">

            <div class="level-left"/>

            <div class="level-right">
              <div class="container">
                <div
                  class="view-btns field is-grouped is-invisible-mobile"
                  align="right">
                  <p class="control">
                    <a
                      class="button view-btn"
                      @click="savePostsView('post-view-a')">_A</a>
                    <a
                      class="button view-btn"
                      @click="savePostsView('post-view-b')">_B</a>
                    <a
                      class="rounded view-btn button"
                      @click="savePostsView('post-view-c')">_C</a>
                  </p>
                </div>
              </div>
            </div>
          </div>
                        
          <div class="columns is-centered">
            <div class="column">
              <nav class="level is-mobile">
                <div class="level-item had-text-centered">
                  <span
                    class="button post-btn is-medium"
                    @click="showEditor()">New Post
                  </span>
                </div>
              </nav>
            </div>
          </div>
          <div class="columns is-centered">
              <content-editor v-if="editorVisible"/>
          </div>
        </div>
      </section>
      <section class="section">
        <keep-alive>
          <component
            :is="postsView"
            :posts="posts"
            v-bind="{getMorePosts}"/>
        </keep-alive>
      </section>

      <div class="level">
        <div class="level-item">
          <button
            id="getMoreBtn"
            class="button"
            v-if="showMoreButton"
            @click="getMorePosts()">Get more..</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { db } from '../../main.js'
// Stages
import PostViewA from '../views/PostViewA'
import PostViewB from '../views/PostViewB'
import PostViewC from '../views/PostViewC'
import ContentEditor from '../stages/ContentEditor'

export default {
  components: {
    PostViewA,
    PostViewB,
    PostViewC,
    ContentEditor
  },
  data () {
    return {
      isLoading: false,
      showMoreButton: true,
      editorVisible: false
    }
  },
  computed: {
    posts () {
      return this.$store.getters.summaries(250)
    },

    postsView () {
      return this.$store.getters.postsView
    }
  },
  mounted () {
    this.getPosts()
  },
  methods: {
    showEditor () {
      this.editorVisible = !this.editorVisible
    },
    deletePost (key, id) {
      this.$store.dispatch('deletePost', { index: key, id })
    },
    initLoading () {
      this.loadingComponent = this.$loading.open({
        container: this.$refs.postsstage.$el
      })
    },
    getPosts () {
      this.$store.dispatch('getPosts')
    },
    getMorePosts () {
      this.$store.dispatch('getMorePosts')
    },
    savePostsView (value) {
      this.$store.dispatch('savePostsView', value)
    }
  }
}
</script>

<style>
.view-btns{
  float: right;
}

.no-overflow {
  overflow: hidden;
}

b-icon{
  margin-top: 2px;
}

.post-btn{
    font: "Proxima N W01 Smbd",Helvetica,Arial,sans-serif;
    color: #fff;
    display: inline-block;
    background: #3e30ed;
    border-radius: 30px;
    font-weight: 500;
    min-width: 214px;
    padding-left: 30px;
    padding-right: 30px;
    padding-top: 15px;
    padding-bottom: 40px;
    text-decoration: none;
    cursor: pointer;
    justify-content: center;
    }
  .post-btn:hover{
      color: #fff;
}

.view-btn{
  font: "Proxima N W01 Smbd",Helvetica,Arial,sans-serif;
  color: #fff;
  font-weight: 500;
  background: #3e30ed;
  border-radius: 30px;
  padding: 20px;
  text-decoration: none;
  cursor: pointer;
}
.view-btn:hover {
  color:#fff;
}

</style>
