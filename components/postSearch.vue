<template>
  <div class="row mx-1">
    <div class="col-sm-6">
      <button
        type="button"
        class="btn btn-primary my-2"
        data-toggle="modal"
        data-target="#postCandidate"
      >
        Post Candidate
      </button>
    </div>
    <div class="col-sm-6">
      <input
        class="form-control my-2"
        type="search"
        placeholder="Search Candidates"
        aria-label="Search"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'postSearch'
}
</script>

<style></style>
