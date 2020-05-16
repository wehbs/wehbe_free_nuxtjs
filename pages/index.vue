<template>
  <div class="d-flex flex-column h-100 wrapper">
    <div class="container">
      <NavBar />
      <h1>THIS IS THE HOME PAGE.</h1>
    </div>
    <Footer />
  </div>
</template>

<script>
import NavBar from '../components/NavBar.vue'
import Footer from '../components/Footer.vue'

export default {
  name: 'Home',
  components: {
    NavBar,
    Footer
  }
}
</script>

<style scoped>
.wrapper {
  min-height: 100vh;
}
</style>
