<script>
    import axios from 'axios';
    export default { 

        data() {
            return {
                    moods: ['Happy', 'Sad', 'Angry'],
                    subject: "",
                    entry: "",
                }
        },
        methods: {
            addPost() {
                axios.get(`${this.baseUrl}/addPost`)
            }
        },
        created() { // created is a hook that executes as soon as Vue instance is created
            axios.get('http://localhost:3000/posts')
            .then(response => {
                this.posts = response.data
            })
            .catch(error => {
                this.posts = [{ entry: 'There was an error: ' + error.message }]
            })
        }
    }
</script>

<template>
    <div class="table m-2">
        <h3>Add a New Blog Post</h3>

        Subject: <input type='text' size='30' v-model='subject' required>
        <br>

        Entry: <br>
        <textarea name='entry' cols='80' rows='5' v-model='entry' required></textarea>
        <br>

        Mood:
        <!-- TODO: Build a dropdown list here for selecting the mood -->
        <select id="mood-select">
            <option value="" disabled>Select mood</option>
            <option v-for="mood in moods">{{ mood }}</option>
        </select>

        <br>

        <br>
        <button>Submit New Post</button>

        <hr> Click  <a><router-link to="/ViewPosts/">here</router-link></a>  to return to Main Page
       
    </div>
</template>

