<template>
  <vs-row
    vs-justify="center"
    vs-align="center"
    class="full-height login-register-bg"
  >
    <vs-col
      type="flex"
      vs-justify="center"
      vs-align="center"
      class="login-register-box"
      vs-lg="12"
      vs-xs="12"
    >
      <vs-card class=" mb-0">
        <div slot="header">
          <h3 class="mb-1">{{ msg }}</h3>
          <p class="mb-0">Welcome back, please login to your account.</p>
        </div>
        <vs-alert
          v-if="errors.length"
          class="mb-3 mt-2"
          color="danger"
          :active="inputValid"
        >
          <b>Please correct the following error(s):</b>
          <ul class="common-list">
            <li v-for="error in errors" :key="error">{{ error }}</li>
          </ul>
        </vs-alert>
        <form @submit="checkForm" novalidate="true">
          <vs-input
            icon-after="true"
            size="large"
            class="w-100 mb-4 mt-2 vs-input-large"
            icon="person"
            placeholder="Email ID"
            v-model="email"
            type="email"
          />
          <vs-input
            icon-after="true"
            size="large"
            class="w-100 vs-input-large"
            type="password"
            icon="lock"
            placeholder="Password"
            v-model="pwd"
          />
          <div class="d-flex pt-3 pb-3">
            <vs-checkbox v-model="rememberme">Remember me?</vs-checkbox>
            <router-link to="/forgotpassword" class="ml-auto"
              >Forgot Password?
            </router-link>
          </div>
          <input
            class="btn-block btn btn-primary submit-button"
            type="submit"
            @click.stop.prevent="handleLogin"
            value="Login"
          />
        </form>
        <div class="d-flex justify-content-center mt-3">
          Don't have an account? &nbsp;
          <router-link to="/register"> Create an Account</router-link>
        </div>
      </vs-card>
    </vs-col>
  </vs-row>
</template>

<script>
export default {
  name: "Login",
  data: () => ({
    msg: "Login Here",
    rememberme: false,
    email: "",
    pwd: "",
    errors: [],
  }),
  methods: {
    handleLogin() {
      if (this.checkForm()) {
        this.$store
          .dispatch("auth/login", { email: this.email, password: this.pwd })
          .then(() => {
            this.$router.push("/units");
          })
          .catch(() => {
            this.errors.push("Invalid email or password.");
          });
      }
    },
    checkForm() {
      this.errors = [];
      if (!this.email) {
        this.errors.push("Email required.");
      } else if (!this.validEmail(this.email)) {
        this.errors.push("Valid email required.");
      }
      if (!this.pwd) this.errors.push("Password required.");
      if (!this.errors.length) return true;
    },
    /* eslint-disable */
    validEmail(email) {
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/;
      return re.test(email);
    },
    /* eslint-enable */
  },
  computed: {
    inputValid() {
      if (this.validEmail(this.email) && this.pwd && !this.errors.length) {
        return false;
      } else {
        return true;
      }
    },
  },
};
</script>
<style>
.login-register-bg {
  margin: 0 auto;
  background: url(../../assets/images/background/auth-bg.jpg) no-repeat center
    center;
}
.login-register-box {
  max-width: 400px;
}
</style>
