<template>
  <div>
    <div v-if="postData[0]" class="row">
      <div class="col-sm-auto mx-auto">
        <div class="card my-2" style="max-width: 40rem">
          <div class="card-body">
            <div class="row no-gutters">
              <div class="col-sm-3">
                <button
                  type="button"
                  class="btn btn-primary my-2"
                  data-toggle="modal"
                  data-target="#postCandidate"
                >
                  Post Candidate
                </button>
              </div>
              <div class="col-sm-9">
                <div class="input-group my-2">
                  <input
                    id="search"
                    v-model="search"
                    type="text"
                    class="form-control"
                    placeholder="Search Candidates"
                    aria-label="Search"
                    aria-describedby="button-addon"
                  />
                  <div class="input-group-append">
                    <button
                      id="search"
                      class="btn btn-outline-primary"
                      type="button"
                      @click="searchPosts"
                    >
                      <i class="fas fa-search"></i>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
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
import loadingIcon from '../components/loadingIcon.vue'
const db = firebase.firestore()

export default {
  name: 'Index',
  components: {
    card,
    modal,
    loadingIcon
  },
  data() {
    return {
      search: '',
      postData: []
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
    },
    searchPosts() {
      const vm = this
      const noCapsSearch = vm.search.toLowerCase().trim()

      db.collection('posts')
        .where('company', '==', noCapsSearch)
        .orderBy('timestamp')
        // .limit(2)
        .get()
        .then(function(querySnapshot) {
          if (vm.search === '') {
            vm.postData = []
            vm.readPosts()
          }
          if (querySnapshot.empty === false) {
            vm.postData = []
          }
          querySnapshot.forEach(function(doc) {
            vm.postData.unshift({
              id: doc.id,
              email: doc.data().email,
              first_name: doc.data().first_name,
              last_name: doc.data().last_name,
              linkedin: doc.data().linkedin,
              refer_pitch: doc.data().refer_pitch,
              date: doc
                .data({ serverTimestamps: 'estimate' })
                .timestamp.toDate()
                .toDateString()
            })

            // const lastVisible =
            //   querySnapshot.docs[querySnapshot.docs.length - 1]
            // console.log('last', lastVisible)

            // function lazyLoad() {
            //   const scrollIsAtTheBottom =
            //     document.documentElement.scrollHeight - window.innerHeight ===
            //     window.scrollY
            //   if (scrollIsAtTheBottom) {
            //     ;(function() {
            //       db.collection('posts')
            //         .where('company', '==', noCapsSearch)
            //         .orderBy('timestamp')
            //         .startAfter(lastVisible)
            //         .limit(2)
            //         .get()
            //         .then(function(querySnapshot) {
            //           querySnapshot.forEach(function(doc) {
            //             vm.postData.unshift({
            //               id: doc.id,
            //               email: doc.data().email,
            //               first_name: doc.data().first_name,
            //               last_name: doc.data().last_name,
            //               linkedin: doc.data().linkedin,
            //               refer_pitch: doc.data().refer_pitch,
            //               date: doc
            //                 .data({ serverTimestamps: 'estimate' })
            //                 .timestamp.toDate()
            //                 .toDateString()
            //             })
            //           })
            //         })
            //     })()
            //   }
            // }
            // window.addEventListener('scroll', lazyLoad)

            console.log(doc.id, ' => ', doc.data())
          })
        })
        .catch(function(error) {
          console.log('Error getting documents: ', error)
        })
    }
  }
}
</script>

<style scoped>
.card {
  border-radius: 2px;
  border: none;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 3px rgba(0, 0, 0, 0.2);
}
</style>
