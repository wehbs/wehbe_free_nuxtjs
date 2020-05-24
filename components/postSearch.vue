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
                @submit.prevent="searchPosts"
              >
                <i class="fas fa-search"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PostSearch',
  data() {
    return {}
  },
  methods: {
    searchPosts() {
      const vm = this
      db.collection('posts')
        .where('company', '==', vm.search)
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

<style scoped>
.card {
  border-radius: 2px;
  border: none;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 3px rgba(0, 0, 0, 0.2);
}
</style>
