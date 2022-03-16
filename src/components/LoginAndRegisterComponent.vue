<template>
  <div id="login-page">
    <div id="input" v-show="!this.$root.$data.isLoggedIn">
      <div id="username-wrapper">
        <input type="text" id="username" placeholder="Username" />
      </div>
      <div></div>
      <div id="password-wrapper">
        <input type="password" id="password" placeholder="Password" />
      </div>
      <button id="login-button" @click="login">Login</button>
      <button id="register-button" @click="register">Register</button>
    </div>
    <div v-show="this.$root.$data.isLoggedIn">
      You are logged in. Enjoy browsing
      <div>
        <button id="logout-button" @click="logout">Logout</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoginAndRegisterComponent",
  methods: {
    login() {
      let username = document.getElementById("username").value;
      let password = document.getElementById("password").value;
      let user = this.$root.$data.users.find(
        (user) => user.username === username
      );
      if (user === undefined) {
        alert("User not found! Please register");
        return;
      }
      if (user.password !== password) {
        alert("Wrong info provided!");
        return;
      }
      this.$root.$data.isLoggedIn = true;
      this.$root.$data.currentUser = username;
      this.$root.$data.currentFavorites = this.$root.$data.savedByUsers.find(
        (u) => u.username === username
      ).favorites;

      document.getElementById("username").value = "";
      document.getElementById("password").value = "";
    },
    register() {
      let newUsername = document.getElementById("username").value;
      let newPassword = document.getElementById("password").value;
      if (newUsername === "" || newPassword === "") {
        alert("Username and password cannot be blank");
        return;
      }
      let user = this.$root.$data.users.find(
        (user) => user.username === newUsername
      );
      if (user !== undefined) {
        alert("User already exists!");
        return;
      }
      this.$root.$data.users.push({
        username: newUsername,
        password: newPassword,
      });
      this.$root.$data.savedByUsers.push({
        username: newUsername,
        favorites: [],
      });
      alert("New user successfully registered");
    },
    logout() {
      this.$root.$data.savedByUsers.find(
        (u) => u.username === this.$root.$data.currentUser
      ).favorites = this.$root.$data.currentFavorites; //save favs before logging out

      this.$root.$data.currentUser = "";
      this.$root.$data.currentFavorites = [];
      this.$root.$data.currentDate = "";
      this.$root.$data.isLoggedIn = false;
    },
  },
};
</script>

<style scoped>
#login-page {
  margin-top: 125px;
}
#username-wrapper,
#password-wrapper {
  margin-bottom: 20px;
}

#username,
#password {
  border: 2px solid gray;
}

#login-button,
#register-button {
  margin: 0 10px;
}

#logout-button {
  margin: 20px 10px;
}
</style>
