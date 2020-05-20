<template>
  <div>
    <div v-for="i in postData" :key="i.id">
      <div class="card mb-2" style="max-width: 40rem">
        <div class="row no-gutters">
          <div class="col-md-4">
            <img
              src="https://tekrabuilders.com/wp-content/uploads/2018/12/male-placeholder-image-768x768.jpeg"
              class="card-img"
              alt="..."
            />
          </div>
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title">
                {{ i.first_name + ' ' + i.last_name }}
              </h5>
              <p class="card-text">
                {{ i.email }}
              </p>
              <a :href="i.linkedin" target="_blank" class="card-link">
                LinkedIn
              </a>
              <p class="card-text">
                <small class="text-muted">{{ i.date }}</small>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    postData: String
  }
}
</script>

<style>
.card {
  border-radius: 2px;
  border: none;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 3px rgba(0, 0, 0, 0.2);
}
</style>
