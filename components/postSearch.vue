<template>
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
              type="text"
              class="form-control"
              placeholder="Search Candidates"
              aria-label="Search"
              aria-describedby="button-addon"
            />
            <div class="input-group-append">
              <button class="btn btn-outline-primary" type="button" id="search">
                <i class="fas fa-search"></i>
              </button>
            </div>
          </div>

          <!-- <input
            class="form-control my-2"
            type="search"
            placeholder="Search Candidates"
            aria-label="Search"
          /> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'postSearch'
}
</script>

<style scoped>
.card {
  border-radius: 2px;
  border: none;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 3px rgba(0, 0, 0, 0.2);
}
</style>
