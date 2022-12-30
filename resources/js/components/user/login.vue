<template>
  <div>
    <form>
      <div class="form-field">
        <input
          id="login-mail"
          autocomplete="email"
          class="form-control"
          type="email"
          name="test"
          v-bind:placeholder="$t('form.email')"
          v-model="email"
        />
        <p class="error" v-for="error in this.error.email">{{ error }}</p>
      </div>
      <div class="form-field">
        <input
          id="login-password"
          class="form-control"
          autocomplete="new-password"
          type="password"
          v-bind:placeholder="$t('form.password')"
          v-model="password"
        />
        <p class="error" v-for="error in this.error.password">{{ error }}</p>
      </div>

      <div class="form-field">
        <button class="btn btn-primary" id="btn-login" v-on:click="login" value="default">
          {{ $t("form.login") }}
        </button>
      </div>
    </form>
  </div>
</template>
<style lang="scss">
  #btn-login {
    border-radius: 10px;

  }
</style>
<script>
export default {
  name: "Login",
  data() {
    return {
      error: "",

      email: "",
      password: "",
      message: "",
      show: false,
    };
  },

  methods: {
    AuthProvider(provider, event) {
      event.preventDefault();
      var self = this;

      this.$auth
        .authenticate(provider)
        .then((response) => {
          self.SocialLogin(provider, response);
        })
        .catch((err) => {
          console.log({ err: err });
        });
    },
    SocialLogin(provider, response) {
      axios
        .post("/api/auth/" + provider, response)
        .then(({ data }) => {
          console.log(data);
          this.$store.commit("user/setUser", data);
          this.$store.commit("user/setAuth", true);
          localStorage.setItem("token", data.user.api_token);
          axios.interceptors.request.use(
            (config) => {
              config.headers["Authorization"] = "Bearer " + data.user.api_token;
              return config;
            },

            (error) => {
              return Promise.reject(error);
            }
          );
          //this.$router.push('/');
        })
        .catch((err) => {
          console.log({ err: err });
        });
    },
    login(e) {
      e.preventDefault();
      let data = {
        email: this.email,
        password: this.password,
      };

      axios
        .post("/api/login", data)
        .then(({ data }) => {
          this.$store.commit("user/setUser", data);
          this.$store.commit("user/setAuth", true);
          localStorage.setItem("token", data.user.api_token);
          axios.interceptors.request.use(
            (config) => {
              config.headers["Authorization"] = "Bearer " + data.user.api_token;
              return config;
            },

            (error) => {
              return Promise.reject(error);
            }
          );
          //this.$router.push('/');
        })
        .catch(({ response }) => {
          this.show = true;
          this.error = response.data.errors;
        });
    },
  },
};
</script>
