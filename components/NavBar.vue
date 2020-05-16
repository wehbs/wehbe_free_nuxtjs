<template>
  <nav class="navbar navbar-light bg-white">
    <nuxt-link to="/">
      <a id="brand" class="navbar-brand">wehbe</a>
    </nuxt-link>
    <div class="navbar-nav d-flex flex-row justify-content-end">
      <nuxt-link to="/signup">
        <button id="signUp" type="button" class="btn btn-outline-* mr-2">
          Join Now
        </button>
      </nuxt-link>
      <nuxt-link to="/signin">
        <button
          id="signIn"
          type="button"
          class="btn btn-outline-primary pl-4 pr-4"
        >
          Sign in
        </button>
      </nuxt-link>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar'
}
</script>

<style scoped>
#brand {
  font-size: 30px;
  font-weight: bold;
}

#signUp {
  color: #666666;
}

#signUp:hover {
  background-color: #f3f3f3;
  color: #3d3d3d;
}

button {
  font-weight: bold;
}
</style>
