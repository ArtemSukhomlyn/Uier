<template>
  <div id="login">
    <form class="form-signin" @submit.prevent="login">
      <img class="mb-4" src="/static/logo.png" alt width="72" height="72">
      <h1 class="h3 mb-3 font-weight-normal">Log on to Uier</h1>
      <input
        type="email"
        id="inputEmail"
        class="form-control"
        placeholder="Email address"
        v-model="email"
        required
        autofocus
      >
      <input
        type="password"
        id="inputPassword"
        class="form-control"
        placeholder="Password"
        v-model="password"
        required
      >
      <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
      <p class="mt-2 text-danger" v-if="error">{{error}}</p>
      <p class="mt-2 text-muted">
        <a href="#">Reset password</a>
      </p>
      <p class="mt-5 text-muted">&nbsp;</p>
    </form>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      email: "admin@uier.com",
      password: "password",
      error: null
    };
  },
  mounted() {},
  methods: {
    login() {
      var parent = this;
      fetch(
        "http://localhost:8081/login?username=" +
          this.email +
          "&password=" +
          this.password,
        {
          credentials: "include",
          method: "POST",
          headers: new Headers({
            "Content-Type": "application/json"
          })
        }
      )
        .then(function(response) {
          return response.json();
        })
        .then(function(json) {
          if (json.status == "authenticated") {
            parent.$root.$data.user = json.user;
            parent.$root.$data.roles = json.roles;
            parent.$root.$data.isAuthenticated = true;
          } else {
            parent.password = "";
            parent.error = "Bad email address or password, or no account exists.";
            parent.password.focus();
          }
        });
    }
  }
};
</script>

<style>
#login {
  text-align: center;
  width: 100%;
  height: 100%;
}

#login {
  display: -ms-flexbox;
  display: flex;
  -ms-flex-align: center;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

body {
  display: -ms-flexbox;
  display: flex;
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
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
