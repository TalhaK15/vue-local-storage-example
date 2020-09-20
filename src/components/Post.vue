<template>
  <v-card outlined elevation="24" style="border: 1px solid grey">
    <v-img :src="image"></v-img>
    <v-card-title>{{title}}</v-card-title>
    <v-card-subtitle>{{date}}</v-card-subtitle>
    <v-card-text>{{text}}</v-card-text>
    <v-card-actions>
      <v-container>
        <v-row>
          <v-col xs="12" sm="12" md="6" cols="6">
            <v-btn @click="updatePost()" color="green darken-1 white--text">Update</v-btn>
          </v-col>
          <v-spacer />
          <v-col cols="6">
            <v-btn @click="deletePost()" color="red darken-1 white--text">Delete</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-card-actions>
  </v-card>
</template>

<script>
import { eventBus } from "../main"
export default {
  name: "Post",
  props: {
    image: String,
    title: String,
    date: String,
    text: String,
  },
  computed: {
    thisPost() {
      return {
        title: this.title,
        text: this.text,
        image: this.image,
        date: this.date,
      }
    },
  },
  methods: {
    updatePost() {
      eventBus.$emit("postContent", this.thisPost)
    },
    deletePost() {
      let posts = JSON.parse(localStorage.getItem("posts"))
      let postIndex = posts.findIndex((p) => {
        return JSON.stringify(p) == JSON.stringify(this.thisPost)
      })

      posts.splice(postIndex, 1)
      localStorage.setItem("posts", JSON.stringify(posts))
      this.$emit("updatePosts", posts)
    },
  },
}
</script>
