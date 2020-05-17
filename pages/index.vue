<template>
  <div class="d-flex flex-column h-100">
    <div class="container">
      <card />
      <h1 v-if="postData[0]">{{ postData[0].email }}</h1>
      <h1 v-else>Loading...</h1>
    </div>
  </div>
</template>

<script>
import card from '../components/card.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card
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
  mounted() {
    this.readPosts()
  },
  methods: {
    readPosts() {
      // const postData = []
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
            console.log(this.postData[0].email)
          })
          // return postData
        })
        .catch((error) => {
          console.log('Error getting documents: ', error)
        })
    }
  }
}
</script>

<style scoped></style>
