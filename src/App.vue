<template>
  <div id="app">
    <nav>
      <router-link to="/" class="route">
        <div class="navigation__logo">
          David's Vue Skills Show - Twooter
        </div>
      </router-link>
      <div class="navigation__user" v-if="state.user">
        {{ state.user.username }}
      </div>
    </nav>
    <router-view />
  </div>
</template>

<script>
import { reactive } from "vue";

export default {
  name: "App",
  components: {},
  setup() {
    const state = reactive({
      user: {
        username: "__DavidDong"
      }
    });
    return { state };
  }
  // data() {
  //   return {
  //     user: {
  //       username: "__DavidDong"
  //     }
  //   };
  // }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background-color: #f3f5fa;
}
nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 5%;
  background-color: darkred;
  color: white;
}
.navigation__logo {
  font-weight: bold;
  font-size: 24px;
  color: white;
}
.route {
  text-decoration: none;
}
.navigation__user {
  font-weight: bold;
}
</style>
