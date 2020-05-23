<template>
  <div class="row justify-content-center">
    <div v-if="postData[0]" class="col-sm-auto">
      <post-search />
      <card :post-data="postData"></card>
    </div>
    <h1 v-else>Loading...</h1>
    <modal />
  </div>
</template>

<script>
import card from '../components/card.vue'
import modal from '../components/modal.vue'
import postSearch from '../components/postSearch.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card,
    modal,
    postSearch
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
      db.collection('posts')
        .orderBy('timestamp', 'desc')
        .onSnapshot(function(snapshot) {
          snapshot.docChanges().forEach(function(change) {
            if (change.type === 'added') {
              vm.postData.push({
                id: change.doc.id,
                email: change.doc.data().email,
                first_name: change.doc.data().first_name,
                last_name: change.doc.data().last_name,
                linkedin: change.doc.data().linkedin,
                refer_pitch: change.doc.data().refer_pitch,
                date: change.doc
                  .data()
                  .timestamp.toDate()
                  .toDateString()
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
