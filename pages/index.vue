<template>
  <div class="row vh-100">
    <div v-if="postData[0]" class="col-sm-auto mx-auto">
      <post-search />
      <card :post-data="postData"></card>
    </div>
    <svg
      v-else
      class="m-auto"
      xmlns:svg="http://www.w3.org/2000/svg"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      version="1.0"
      width="64px"
      height="64px"
      viewBox="0 0 128 128"
      xml:space="preserve"
    >
      <rect x="0" y="0" width="100%" height="100%" fill="#f5f5f5" />
      <g>
        <circle cx="16" cy="64" r="16" fill="#000000" fill-opacity="1" />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#555555"
          fill-opacity="0.67"
          transform="rotate(45,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#949494"
          fill-opacity="0.42"
          transform="rotate(90,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#cccccc"
          fill-opacity="0.2"
          transform="rotate(135,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#e1e1e1"
          fill-opacity="0.12"
          transform="rotate(180,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#e1e1e1"
          fill-opacity="0.12"
          transform="rotate(225,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#e1e1e1"
          fill-opacity="0.12"
          transform="rotate(270,64,64)"
        />
        <circle
          cx="16"
          cy="64"
          r="16"
          fill="#e1e1e1"
          fill-opacity="0.12"
          transform="rotate(315,64,64)"
        />
        <animateTransform
          attributeName="transform"
          type="rotate"
          values="0 64 64;315 64 64;270 64 64;225 64 64;180 64 64;135 64 64;90 64 64;45 64 64"
          calcMode="discrete"
          dur="720ms"
          repeatCount="indefinite"
        ></animateTransform>
      </g>
    </svg>
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
