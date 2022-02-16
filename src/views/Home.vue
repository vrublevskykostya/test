<template>
    <div class="container">
      <PostsList />
    </div>

</template>

<script>
import PostsList from "../components/PostsList";

export default {
  name: 'App',
  data() {
    return {
      posts: [],
    }
  },
  components: {
    PostsList
  },
}
</script>

<style>


</style>

