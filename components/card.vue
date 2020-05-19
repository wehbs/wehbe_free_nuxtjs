<template>
  <div>
    <div v-for="i in postData" :key="i.id" class="row justify-content-center">
      <div class="card mt-1" style="max-width: 540px;">
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
                This is a wider card with supporting text below as a natural
                lead-in to additional content. This content is a little bit
                longer.
              </p>
              <p class="card-text">
                {{ i.email }}
              </p>
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
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.2);
}
</style>
