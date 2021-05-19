<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ state.user.username }}</h1>
      <!-- v-if and v-else conditional -->
      <div class="user-profile__admin-badge" v-if="state.user.isAdmin">
        Admin
      </div>
      <div class="user-profile__admin-badge" v-else>
        Common User
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong>{{ state.followers }}
        <button class="button" @click="followUser">+Followers</button>
      </div>
      <div class="user-profile__favourite">
        {{ state.favouritedTwoot }}
      </div>
      <!--  @add-twoot="addNewTwootToTwoots", capture the data from child component-->
      <CreateTwootPanel @add-twoot="addNewTwootToTwoots" />
    </div>
    <div class="user-profile__twoots-wrapper">
      <!-- v-for loop and data passing -->
      <!-- @favourite="toggleFavourite" capture the emit date -->
      <TwootItem
        v-for="twoot in state.user.twoots"
        :key="twoot.id"
        :username="state.user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import { reactive,computed } from "vue";
import { useRoute } from "vue-router";
import {users} from "../assets/users"
import TwootItem from "../components/TwootItem";
import CreateTwootPanel from "../components/CreatTwootPanel";
export default {
  name: "UserProfile",
  components: { TwootItem, CreateTwootPanel },
  setup() {
    const route = useRoute();
    // get user id from route
    const userId = computed(()=> route.params.userId);


    const state = reactive({
      followers: 0,
      favouritedTwoot: "",
      // if the userId doesn't exist, use user 1
      user: users[userId.value -1] || users[0]
    });
    function followUser() {
      state.followers++;
    }
    function toggleFavourite(id) {
      state.favouritedTwoot = `My favorite twoot's id is ${id}`;
    }
    function addNewTwootToTwoots(newTwoot) {
      state.user.twoots.unshift({
        id: state.user.twoots.length + 1,
        content: newTwoot
      });
    }

    return { state, followUser, toggleFavourite, addNewTwootToTwoots, userId };
  }
};
// data() {
//   return {
//     followers: 0,
//     favouritedTwoot: "",
//     user: {
//       id: 1,
//       username: "_DavidDong",
//       firstName: "David",
//       lastName: "Dong",
//       email: "daviddong201612@gmail.com",
//       isAdmin: false,
//       twoots: [
//         { id: 1, content: "Learing Vue.js" },
//         { id: 2, content: "Learing NodeJs" },
//         { id: 3, content: "Learing Java" },
//         { id: 4, content: "Learing MySQL" }
//       ]
//     }
//   };
// },
// watch: {
//   followers(newFollowerCount, oldFollowerCount) {
//     if (oldFollowerCount < newFollowerCount) {
//       console.log(`${this.user.username} has gained a follower!`);
//     }
//   }
// },

// methods: {
//   followUser() {
//     this.followers++;
//   },
//   toggleFavourite(id) {
//     this.favouritedTwoot = `My favorite twoot's id is ${id}`;
//   },
//   addNewTwootToTwoots(newTwoot){
//     this.user.twoots.unshift(
//       {id:this.user.twoots.length+1, content:newTwoot})
//   }

// },
// mounted() {
//   this.followUser();
// }
</script>

<style scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}
.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px sold #dfe3e8;
}

.user-profile__admin-badge {
  background: darkred;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  padding: 0 10px;
  font-weight: bold;
}

.user-profile__favourite {
  background: darkgreen;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  margin:10px;
  padding: 0 20px;
}
.user-profile__twoots-wrapper {
  display: grid;
  grid-gap: 10px;
}

h1 {
  margin: 0;
}
.button {
  background: darkgreen;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  margin-left: 20px;
  margin-top: 20px;
  padding: 0 20px;
}
</style>
