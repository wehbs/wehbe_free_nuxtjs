<template>
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
              <label for="referPitch">Why refer you?</label>
              <textarea
                id="referPitch"
                v-model="refer_pitch"
                type="text"
                class="form-control"
                rows="3"
                required
              ></textarea>
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
  <!-- End Modal -->
</template>

<script>
const db = firebase.firestore()

export default {
  name: 'Modal',
  data() {
    return {
      email: '',
      first_name: '',
      last_name: '',
      linkedin: '',
      refer_pitch: '',
      date:
        new Date().toLocaleDateString() +
        ' at ' +
        new Date().toLocaleTimeString()
    }
  },
  methods: {
    createPost() {
      db.collection('posts')
        .add({
          email: this.email,
          first_name: this.first_name,
          last_name: this.last_name,
          linkedin: this.linkedin,
          refer_pitch: this.refer_pitch,
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
      this.refer_pitch = ''
    }
  }
}
</script>

<style></style>
