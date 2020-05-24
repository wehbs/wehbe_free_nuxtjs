<template>
  <div>
    <div v-for="i in postData" :key="i.id">
      <div class="card mb-2" style="max-width: 40rem">
        <div class="row no-gutters">
          <div class="card-body">
            <div class="col">
              <div class="row">
                <div class="col-auto">
                  <img
                    src="https://media-exp1.licdn.com/dms/image/C5603AQHokXt4KfwQ4A/profile-displayphoto-shrink_400_400/0?e=1595462400&v=beta&t=Pw7nZrbClhWRTmuOz2BFMopQUcmWS-8uushGJRSTk3w"
                    class="card-img"
                    alt="..."
                  />
                </div>
                <div class="col">
                  <h5 class="card-title">
                    {{ i.first_name + ' ' + i.last_name }}
                  </h5>
                  <h6 class="card-subtitle mb-2 text-muted">lorem ipsum</h6>
                </div>
              </div>
              <div class="row">
                <div class="col">
                  <p class="card-text">
                    {{ i.refer_pitch }}
                  </p>
                  <a :href="i.linkedin" target="_blank" class="card-link">
                    LinkedIn
                  </a>
                  <a
                    :href="'mailto:' + i.email"
                    target="_blank"
                    class="card-link"
                  >
                    Email
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

<style scoped>
.card {
  border-radius: 2px;
  border: none;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.15), 0 2px 3px rgba(0, 0, 0, 0.2);
}

img {
  margin: 0;
  padding: 0;
  vertical-align: baseline;
  background: transparent;
  outline: 0;
  width: 48px;
  height: 48px;
  box-sizing: border-box;
  background-clip: content-box;
  border: 2px solid transparent;
  border-radius: 49.9%;
  background-position: 50%;
  object-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  object-fit: contain;
}
</style>
