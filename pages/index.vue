<template>
  <div>
    <div v-if="postData[0]" class="row">
      <div class="col-sm-auto mx-auto">
        <post-search />
        <card :post-data="postData"></card>
      </div>
    </div>
    <div v-else class="row min-vh-100">
      <div class="col-auto m-auto">
        <loadingIcon />
      </div>
    </div>
    <modal />
  </div>
</template>

<script>
import card from '../components/card.vue'
import modal from '../components/modal.vue'
import postSearch from '../components/postSearch.vue'
import loadingIcon from '../components/loadingIcon.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card,
    modal,
    postSearch,
    loadingIcon
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
        .orderBy('timestamp')
        .onSnapshot(function(snapshot) {
          snapshot.docChanges().forEach(function(change) {
            if (change.type === 'added') {
              vm.postData.unshift({
                id: change.doc.id,
                email: change.doc.data().email,
                first_name: change.doc.data().first_name,
                last_name: change.doc.data().last_name,
                linkedin: change.doc.data().linkedin,
                refer_pitch: change.doc.data().refer_pitch,
                date: change.doc
                  .data({ serverTimestamps: 'estimate' })
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
