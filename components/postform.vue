<template>
  <div>
    <button
      type="button"
      class="btn btn-primary mt-2"
      data-toggle="modal"
      data-target="#postCandidate"
    >
      Post Candidate
    </button>

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
            <form id="postForm" v-on:submit.prevent="$emit('callCreatePost')">
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
</template>

<script>
export default {
  name: 'Postform'
}
</script>

<style></style>
