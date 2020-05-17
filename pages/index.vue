<template>
  <div class="d-flex flex-column h-100">
    <div v-if="postData[0]" class="container">
      <postform v-on:callCreatePost="createPost"></postform>
      <card :post-data="postData"></card>
    </div>
    <h1 v-else>Loading...</h1>
  </div>
</template>

<script>
import card from '../components/card.vue'
import postform from '../components/postform.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card,
    postform
  },
  data() {
    return {
      email: '',
      first_name: '',
      last_name: '',
      linkedin: '',
      date: new Date().toISOString().slice(0, 10),
      postData: [],
      search: ''
    }
  },
  beforeMount() {
    this.readPosts()
  },
  methods: {
    readPosts() {
      db.collection('posts')
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            this.postData.push({
              id: doc.id,
              email: doc.data().email,
              first_name: doc.data().first_name,
              last_name: doc.data().last_name,
              linkedin: doc.data().linkedin
            })
            console.log(doc.id, ' => ', doc.data())
            console.log(this.postData[0])
          })
        })
        .catch((error) => {
          console.log('Error getting documents: ', error)
        })
    }
  },
  createPost(email, first_name, last_name, linkedin, date) {
    db.collection('posts')
      .add({
        email: email,
        first_name: first_name,
        last_name: last_name,
        linkedin: linkedin,
        date: date
      })
      .then(function(docRef) {
        console.log('Document written with ID: ', docRef.id)
        this.readPosts()
      })
      .catch((error) => {
        console.error('Error writing document: ', error)
      })
    this.email = ''
    this.first_name = ''
    this.last_name = ''
    this.linkedin = ''
  }
}
</script>

<style scoped></style>
