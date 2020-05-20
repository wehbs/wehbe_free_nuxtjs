<template>
  <div class="row justify-content-center">
    <div class="col-md-5">
      <div v-if="postData[0]">
        <button
          type="button"
          class="btn btn-primary mt-2 mb-2"
          data-toggle="modal"
          data-target="#postCandidate"
        >
          Post Candidate
        </button>

        <card :post-data="postData"></card>
      </div>
      <h1 v-else>Loading...</h1>
      <!-- Modal -->
      <div
        id="postCandidate"
        class="modal fade"
        data-backdrop="static"
        data-keyboard="false"
        tabindex="-1"
        role="dialog"
        aria-labelledby="staticBackdropLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h5 id="staticBackdropLabel" class="modal-title">
                Post Candidate
              </h5>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-label="Close"
              >
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form id="postForm" @submit.prevent="createPost">
                <div class="form-row">
                  <div class="col-md-6 mb-3">
                    <label for="firstName">First name</label>
                    <input
                      id="firstName"
                      v-model="first_name"
                      type="text"
                      class="form-control"
                      required
                    />
                  </div>
                  <div class="col-md-6 mb-3">
                    <label for="lastName">Last name</label>
                    <input
                      id="lastName"
                      v-model="last_name"
                      type="text"
                      class="form-control"
                      required
                    />
                  </div>
                </div>
                <div class="form-row">
                  <div class="col-md-6 mb-3">
                    <label for="email">Email address</label>
                    <input
                      id="email"
                      v-model="email"
                      type="email"
                      class="form-control"
                      required
                    />
                  </div>
                  <div class="col-md-6 mb-3">
                    <label for="linkedIn">LinkedIn URL</label>
                    <input
                      id="linkedIn"
                      v-model="linkedin"
                      type="text"
                      class="form-control"
                      required
                    />
                  </div>
                </div>
                <div class="form-group">
                  <div class="form-check">
                    <input
                      id="invalidCheck2"
                      class="form-check-input"
                      type="checkbox"
                      value=""
                      required
                    />
                    <label class="form-check-label" for="invalidCheck2">
                      Agree to terms and conditions
                    </label>
                  </div>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button type="submit" class="btn btn-primary" form="postForm">
                Post
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Modal -->
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
    },
    createPost() {
      db.collection('posts')
        .add({
          email: this.email,
          first_name: this.first_name,
          last_name: this.last_name,
          linkedin: this.linkedin,
          date: this.date
        })
        .then(function(docRef) {
          console.log('Document written with ID: ', docRef.id)
          $('#postCandidate').modal('hide')
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
}
</script>

<style scoped></style>
