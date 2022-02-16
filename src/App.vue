<template>
  <div id="app">
    <div class="container" >
      <div class="show-item" v-show="showItem" @click.prevent="showItem= false">
        <h1 class=""> {{ name }} </h1>
        <p> {{ body }} </p>
      </div>
      <div class="d-flex ">
        <h1>Codeigniter 3 GRUD Example from scratch</h1>
        <button class="btn btn-success" @click="showDialog=!showDialog">Create New Item</button>
      </div>

      <div class="card" v-show="showDialog">
        <form action="">
          <input class="form-control m-2" type="text" placeholder="title" v-model="name">
          <input class="form-control m-2" type="text" placeholder="description" v-model="body">
          <button v-if="buttonSelected" class="btn btn-info m-2" @click.prevent="addNewPost">Add Item</button>
          <button v-else class="btn btn-info m-2" @click.prevent="editPost">Edit Item</button>
        </form>
      </div>

      <table class="table">
        <thead>
        <tr>
          <th>name</th>
          <th>body</th>
          <th>Action</th>
       </tr>
        </thead>
        <tbody>
        <tr
            v-for="(post, i) in posts"
            :key="post.id"
        >
          <td> {{ post.name }} </td>
          <td> {{ post.body }} </td>
          <td><button class="btn btn-info" @click.prevent="show(i)">Show</button></td>
          <td><button class="btn btn-primary" @click.prevent="showEditPost(i)">Edit Item</button></td>
          <td><button class="btn btn-danger" @click.prevent="deletePost(i)">Delete Item</button></td>
        </tr>

        </tbody>
      </table>
    </div>

  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      showItem: false,
      buttonSelected: true,
      showDialog: false,
      posts: [],
      name: '',
      body: ''
    }
  },
  async mounted() {
    const res = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=3')
    this.posts = await res.json()
    console.log(this.posts)
  },
  methods: {
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
      this.buttonSelected = false,
      this.showDialog = true
      this.name = this.posts[i].name
      this.body = this.posts[i].body
    },
    editPost(i){
      const editPost = {
        name: this.name,
        body: this.body
      }
      this.posts.splice(i,1, editPost)
      this.name = this.body = ''
      this.showDialog = false
    },
    show(i) {
      this.showItem = true
      this.name = this.posts[i].name
      this.body = this.posts[i].body
    }
  }
}
</script>

<style>
.show-item {
  display: block;
  max-width: 600px;
  margin: 0 auto;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  padding: 1rem;
}

</style>
