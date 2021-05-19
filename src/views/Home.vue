<template>
  <div class="home">
    <h1>Home</h1>
    <div class="users-list">
      <router-link
        v-for="user in users"
        :to="{ name: 'UserProfile', params: { userId: user.id } }"
        :key="user.id"
        class="users-list-item"
      >
        @{{ user.username }}
      </router-link>
    </div>
  </div>
</template>

<script>
import { users } from "../assets/users";

export default {
  name: "Home",
  setup() {
    return {
      users
    };
  }
};
</script>

<style scoped>
.home {
  padding: 50px 5%;
}
.users-list {
  display: flex;
  flex-direction: column;
}
.users-list-item {
  width: 200px;
  padding: 5px;
  margin: 10px;
  background-color: darkred;
  color:white;
  font-weight: bold;
  font-size: 20px;
  text-decoration: none;
  border-radius: 5%;

}
.users-list-item:hover{
  transition: all 0.5s ease;
  transform: scale(2, 2);
}
</style>
