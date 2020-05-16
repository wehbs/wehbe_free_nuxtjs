<template>
  <div class="text-center wrapper d-flex flex-column h-100">
    <img class="mb-4" src="../assets/logo.png" alt width="72" height="72" />
    <h3 class="mb-3 font-weight-bold">Welcome Back</h3>
    <h6 class="mb-3 font-weight-bold text-muted">
      Where Employees Help Bring Employees
    </h6>
    <form class="form-signin" @submit.prevent="submit">
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
        placeholder="Password"
        required
      />
      <button class="btn btn-lg btn-primary btn-block" type="submit">
        Sign in
      </button>
      <p class="mt-3 mb-3" @click="reset">
        <a href="#">Forgot password?</a>
      </p>
      <p class="mt-3 mb-3">
        New to Wehbe?
        <router-link to="/signup">Join Now</router-link>
      </p>
    </form>
    <Footer />
  </div>
</template>

<script>
import Footer from '../components/Footer.vue'
export default {
  name: 'SignIn',
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
        .signInWithEmailAndPassword(this.form.email, this.form.password)
        .then(() => {
          this.$router.push('/loggedin')
        })
        .catch((error) => {
          if (
            error.message ===
            'There is no user record corresponding to this identifier. The user may have been deleted.'
          ) {
            this.error = "Hmm, we don't recognize that email. Please try again."
          } else if (
            error.message ===
            'The password is invalid or the user does not have a password.'
          ) {
            this.error = "Hmm, that's not the right password. Please try again."
          } else {
            this.error = error.message
          }
          // console.log(error)
        })
    },
    reset() {
      firebase
        .auth()
        .sendPasswordResetEmail(this.form.email)
        .then(() => {
          this.error = 'Password Reset Email Sent!'
        })
        .catch((error) => {
          if (error.code === 'auth/user-not-found') {
            this.error =
              "We couldn't find a Wehbe account associated with " +
              this.form.email +
              '.'
          } else {
            this.error = error.message
          }
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

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
.form-signin .checkbox {
  font-weight: 400;
}
.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type='email'] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type='password'] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
