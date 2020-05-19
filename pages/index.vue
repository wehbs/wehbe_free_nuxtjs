<template>
  <div class="row justify-content-center">
    <div class="col-lg-5">
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
      db.collection('posts').onSnapshot(function(querySnapshot) {
        querySnapshot.forEach(function(doc) {
          vm.postData.push({
            id: doc.id,
            email: doc.data().email,
            first_name: doc.data().first_name,
            last_name: doc.data().last_name,
            linkedin: doc.data().linkedin,
            date: doc.data().date
          })
          console.log(doc.id, ' => ', doc.data())
          // console.log(vm.date)
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
      this.postData = []
      this.readPosts()
      this.email = ''
      this.first_name = ''
      this.last_name = ''
      this.linkedin = ''
    }
  }
}
</script>

<style scoped></style>
