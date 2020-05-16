<template>
  <div class="text-center wrapper d-flex flex-column h-100">
    <img class="mb-4" src="../assets/logo.png" alt width="72" height="72" />
    <h3 class="mb-3 font-weight-bold">Where employees help bring employees</h3>
    <form class="form-signup" @submit.prevent="submit">
      <div v-if="error" class="alert alert-danger">{{ error }}</div>

      <input
        id="inputEmail"
        v-model="form.email"
        type="email"
        class="form-control"
        placeholder="Email"
        required
        autofocus
      />
      <label for="inputPassword" class="sr-only">Password</label>
      <input
        id="inputPassword"
        v-model="form.password"
        type="password"
        class="form-control"
        placeholder="Password (6 or more characters)"
        required
      />
      <button class="btn btn-lg btn-primary btn-block" type="submit">
        Join
      </button>
      <p class="mt-3 mb-3">
        Already on Wehbe?
        <router-link to="/signin">Sign in</router-link>
      </p>
    </form>
    <Footer />
  </div>
</template>

<script>
import Footer from '../components/Footer.vue'
export default {
  name: 'SignUp',
  components: {
    Footer
  },
  data() {
    return {
      form: {
        email: '',
        password: ''
      },
      error: null
    }
  },
  methods: {
    submit() {
      firebase
        .auth()
        .createUserWithEmailAndPassword(this.form.email, this.form.password)
        .then(() => {
          this.$router.push('/loggedin')
        })
        .catch((error) => {
          this.error = error.message
          // console.log(error)
        })
    }
  }
}
</script>

<style scoped>
.wrapper {
  min-height: 100vh;
  /* display: -ms-flexbox; */
  /* display: flex; */
  -ms-flex-align: center;
  align-items: center;
  padding-top: 40px;
  background-color: #f5f5f5;
}

.form-signup {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
.form-signup .checkbox {
  font-weight: 400;
}
.form-signup .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signup .form-control:focus {
  z-index: 2;
}
.form-signup input[type='email'] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signup input[type='password'] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
