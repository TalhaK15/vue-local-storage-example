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
              <v-row>
                <v-col cols="6" v-for="(post,i) in posts" :key="i">
                  <Post
                    @updatePosts="updatePosts($event)"
                    :image="post.image"
                    :title="post.title"
                    :date="post.date"
                    :text="post.text"
                  />
                </v-col>
              </v-row>
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
