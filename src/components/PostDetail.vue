<template>
  <div>
    <div v-if="post">
      <h1> {{ post.name }}</h1>
      <p> {{ post.body }}</p>
    </div>
    <div v-else>
      <h1>Post not found</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostDetail",
  props: {
    post: {
      type: Array,
      default: null
    }
  }
}
</script>

<style scoped>

</style>