<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="4">
            <AddPost @updatePosts="updatePosts($event)" />
          </v-col>
          <v-col cols="8" v-if="posts != []">
            <v-container>
              <transition-group name="fade" tag="div" class="row">
                <v-col cols="6" v-for="post in posts" :key="posts.indexOf(post)">
                  <Post
                    @updatePosts="updatePosts($event)"
                    :image="post.image"
                    :title="post.title"
                    :date="post.date"
                    :text="post.text"
                  />
                </v-col>
              </transition-group>
            </v-container>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import AddPost from "./components/AddPost"
import Post from "./components/Post"

export default {
  name: "App",

  components: {
    AddPost,
    Post,
  },
  data: () => ({
    posts: [],
  }),
  methods: {
    updatePosts(e) {
      this.posts = e
    },
  },
  created() {
    let posts = localStorage.getItem("posts")
    if (posts) {
      this.posts = JSON.parse(posts)
    }
  },
}
</script>

<style scoped>
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
</style>