<template>
  <div class="row justify-content-center">
    <div class="col-sm-auto">
      <div v-if="postData[0]">
        <div class="row">
          <div class="col-5">
            <button
              type="button"
              class="btn btn-primary my-2"
              data-toggle="modal"
              data-target="#postCandidate"
            >
              Post Candidate
            </button>
          </div>
          <div class="col-7">
            <input
              class="form-control my-2"
              type="search"
              placeholder="Search Candidates"
              aria-label="Search"
            />
          </div>
        </div>
        <card :post-data="postData"></card>
      </div>
      <h1 v-else>Loading...</h1>
      <modal />
    </div>
  </div>
</template>

<script>
import card from '../components/card.vue'
import modal from '../components/modal.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card,
    modal
  },
  data() {
    return {
      postData: [],
      search: ''
    }
  },
  mounted() {
    this.readPosts()
  },
  methods: {
    readPosts() {
      const vm = this
      db.collection('posts').onSnapshot(function(snapshot) {
        snapshot.docChanges().forEach(function(change) {
          if (change.type === 'added') {
            vm.postData.push({
              id: change.doc.id,
              email: change.doc.data().email,
              first_name: change.doc.data().first_name,
              last_name: change.doc.data().last_name,
              linkedin: change.doc.data().linkedin,
              date: change.doc.data().date
            })
          }
          if (change.type === 'modified') {
            console.log(change.doc.id, ' => ', change.doc.data())
          }
          if (change.type === 'removed') {
            const elementPos = vm.postData
              .map(function(i) {
                return i.id
              })
              .indexOf(change.doc.id)
            if (elementPos !== -1) {
              vm.postData.splice(elementPos, 1)
            }
          }
        })
      })
    }
  }
}
</script>

<style scoped></style>
