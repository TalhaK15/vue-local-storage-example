<template>
  <v-card outlined elevation="24" style="border: 1px solid grey">
    <v-card-title>Add Post</v-card-title>
    <v-card-text>
      <v-form>
        <v-container>
          <v-row>
            <v-text-field v-model="title" label="Title*" outlined></v-text-field>
          </v-row>
          <v-row>
            <v-text-field v-model="text" label="Text*" outlined></v-text-field>
          </v-row>
          <v-row>
            <v-text-field v-model="image" label="Image*" outlined></v-text-field>
          </v-row>
          <v-row>
            <span>Date*</span> :
            <v-date-picker full-width v-model="date" style="border: 2px solid grey"></v-date-picker>
          </v-row>
          <v-row>
            <v-btn
              class="mt-10"
              color="red darken-1 white--text"
              v-if="update === 1"
              @click="clearInputs()"
            >Cancel</v-btn>
            <v-spacer></v-spacer>
            <v-btn
              class="mt-10"
              color="primary"
              :disabled="!valid"
              @click="addPost()"
            >{{ update == 0 ? "Add Post" : "Update"}}</v-btn>
          </v-row>
          <v-row>
            <span class="red--text">*: required</span>
          </v-row>
        </v-container>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script>
import { eventBus } from "../main"
export default {
  name: "AddPost",

  data: () => ({
    required: (value) => !!value || "Required.",
    update: 0,
    updatePost: {},
    title: "",
    text: "",
    image: "",
    date: new Date().toISOString().substr(0, 10),
  }),
  methods: {
    addPost() {
      let newPost = {
        title: this.title,
        text: this.text,
        image: this.image,
        date: this.date,
      }
      if (this.update === 0) {
        if (localStorage.getItem("posts")) {
          let posts = JSON.parse(localStorage.getItem("posts"))
          posts.push(newPost)
          localStorage.setItem("posts", JSON.stringify(posts))
        } else {
          let posts = [newPost]
          localStorage.setItem("posts", JSON.stringify(posts))
        }
      } else {
        let posts = JSON.parse(localStorage.getItem("posts"))
        let postIndex = posts.findIndex((p) => {
          return JSON.stringify(p) == JSON.stringify(this.updatePost)
        })
        posts[postIndex] = newPost
        localStorage.setItem("posts", JSON.stringify(posts))
        this.update = 0
        this.updatePost = {}
      }
      this.$emit("updatePosts", JSON.parse(localStorage.getItem("posts")))
      this.title = ""
      this.text = ""
      this.image = ""
      this.date = new Date().toISOString().substr(0, 10)
    },
    clearInputs() {
      this.update = 0
      this.updatePost = {}
      this.title = ""
      this.text = ""
      this.image = ""
      this.date = new Date().toISOString().substr(0, 10)
    },
  },
  computed: {
    valid() {
      if (this.title && this.text && this.image != "") return true
      else return false
    },
  },
  created() {
    eventBus.$on("postContent", (content) => {
      this.updatePost = {
        title: content.title,
        text: content.text,
        image: content.image,
        date: content.date,
      }
      this.title = content.title
      this.text = content.text
      this.image = content.image
      this.date = content.date
      this.update = 1
    })
  },
}
</script>
