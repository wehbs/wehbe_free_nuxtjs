<template>
  <div id="footerBg" class="mt-auto">
    <footer class="py-4 md-5 pt-md-5 container">
      <div class="row">
        <div class="col-12 col-md">
          <img
            class="mb-2"
            src="../assets/logo.png"
            alt
            width="24"
            height="24"
          />
          <small class="d-block mb-3 text-muted">&copy; 2019-2020</small>
        </div>
        <div class="col-6 col-md">
          <h5>Features</h5>
          <ul class="list-unstyled text-small">
            <li>
              <a class="text-muted" href="#">Cool stuff</a>
            </li>
          </ul>
        </div>
        <div class="col-6 col-md">
          <h5>Resources</h5>
          <ul class="list-unstyled text-small">
            <li>
              <a class="text-muted" href="#">Resource</a>
            </li>
          </ul>
        </div>
        <div class="col-6 col-md">
          <h5>About</h5>
          <ul class="list-unstyled text-small">
            <li>
              <a class="text-muted" href="#">Team</a>
            </li>
          </ul>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style scoped>
#footerBg {
  background-color: #faf9f7;
  /* position: fixed;
  bottom: 0; */
  width: 100%;
}
</style>
