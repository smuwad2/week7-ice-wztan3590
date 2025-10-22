<script>
    import axios from 'axios';
    export default {
    data() {
        return {
        posts: [],
        editPostId: null, // which post is being edited
        editForm: { entry: '', mood: '' },
        uniqueMoods: []
        };
    },
    created() {
        axios.get('http://localhost:3000/posts').then(res => {
        this.posts = res.data;
        this.uniqueMoods = [...new Set(this.posts.map(p => p.mood))];
        });
    },
    methods: {
        startEdit(post) {
        this.editPostId = post.id;
        this.editForm.entry = post.entry;
        this.editForm.mood = post.mood;
        },
        cancelEdit() {
        this.editPostId = null;
        this.editForm.entry = '';
        this.editForm.mood = '';
        },
        saveEdit(id) {
        axios.post(`http://localhost:3000/updatePost?id=${id}`, {
            entry: this.editForm.entry,
            mood: this.editForm.mood
        }).then(() => {
            // Refresh posts list after updating (or update local array)
            return axios.get('http://localhost:3000/posts');
        }).then(res => {
            this.posts = res.data;
            this.cancelEdit();
        });
        }
    }
    }
</script>

<template>
  <table>
    <thead>
      <tr><th>ID</th><th>Entry</th><th>Mood</th><th>Action</th></tr>
    </thead>
    <tbody>
      <tr v-for="post in posts" :key="post.id">
        <td>{{post.id}}</td>
        <template v-if="editPostId === post.id">
          <td><input v-model="editForm.entry"></td>
          <td>
            <select v-model="editForm.mood">
              <option v-for="m in uniqueMoods" :value="m">{{m}}</option>
            </select>
          </td>
          <td>
            <button @click="saveEdit(post.id)">Save</button>
            <button @click="cancelEdit">Cancel</button>
          </td>
        </template>
        <template v-else>
          <td>{{post.entry}}</td>
          <td>{{post.mood}}</td>
          <td><button @click="startEdit(post)">Edit</button></td>
        </template>
      </tr>
    </tbody>
  </table>
</template>
