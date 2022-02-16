<template>
  <div>
    <div class="d-flex ">
      <h1>Codeigniter 3 GRUD Example from scratch</h1>
      <button class="btn btn-success" @click="createPost">Create New Item</button>
    </div>

    <table class="table">
      <thead>
      <tr>
        <th>Title</th>
        <th>Description</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr
          v-for="(post, i) in posts"
          :key="post.id"
      >
        <td>
          <p v-if="!post.editing">{{ post.name }}</p>
          <textarea
              v-else
              v-model="post.name"
              class="no-resize"
              rows="2"
          ></textarea>
        </td>
        <td>
          <p v-if="!post.editing">{{ post.body }}</p>
          <textarea
              v-else
              v-model="post.body"
              class="no-resize"
              rows="2"
          ></textarea>
        </td>
        <td>
          <button :disabled="post.editing" class="btn btn-info" @click.prevent="showPost(post.id)">Show</button>
        </td>
        <td>
          <button v-if="!post.editing" class="btn btn-primary" @click.prevent="post.editing = true">
            Edit
          </button>
          <button v-else class="btn btn-primary" @click.prevent="savePost(i)">
            Save
          </button>
        </td>
        <td>
          <button :disabled="post.editing" class="btn btn-danger" @click.prevent="deletePost(i)">Delete</button>
        </td>
      </tr>

      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      posts: [],
    }
  },
  created() {
    this.getPosts()
  },
  methods: {
    async getPosts() {
      const res = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=3')
      const postsJson = await res.json()
      this.posts = postsJson.map((p) => {
        p.editing = false
        return p
      })
    },
    deletePost(id) {
      this.posts.splice(id, 1)
    },
    addNewPost() {
      this.newPost = {
        name: this.name,
        body: this.body
      }
      this.posts.unshift(this.newPost)
      this.name = this.body = ''
      this.showDialog = false
    },
    showEditPost(i) {
      this.buttonSelected = false
      this.showDialog = true
      this.name = this.posts[i].name
      this.body = this.posts[i].body
    },
    editPost(i) {
      const editPost = {
        name: this.name,
        body: this.body
      }
      this.posts.splice(i, 1, editPost)
      this.name = this.body = ''
      this.showDialog = false
    },
    showPost(postId) {
      this.$router.push({name: 'Detail', params: {postId}})
    },
    closeItem() {
      this.showItem = false,
          this.name = this.body = ''
    },
    createPost() {
      this.posts.unshift({
        name: '',
        body: '',
        editing: true,
        id: +Date.now()
      })
    },
    savePost(i) {
      this.posts[i].editing = false
      localStorage.setItem('posts', JSON.stringify(this.posts))
    }
  }
}
</script>

<style scoped>
.show-item {
  display: block;
  max-width: 600px;
  margin: 0 auto;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  padding: 1rem;
}
.no-resize {
  resize: none;
  width: 100%;
}
</style>