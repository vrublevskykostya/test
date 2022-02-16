<template>
  <div class="about container">
    <h3><router-link to="/">Return to home</router-link></h3>
    <PostDetail :post="post"/>
  </div>
</template>

<script>
import PostDetail from "../components/PostDetail";

export default {
  name: "Detail",
  data() {
    return {
      post: null
    }
  },
  components: {
    PostDetail
  },
  created() {
    const posts = localStorage.getItem('posts') ? JSON.parse(localStorage.getItem('posts')) : []
    if (posts.length) {
      const filteredPost = posts.filter((p) => {
        return +p.id === +this.$route.params.postId
      })
      if (filteredPost.length) {
        this.post = filteredPost[0]
      }
    }
  },

}
</script>
