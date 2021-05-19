<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <!-- v-if and v-else conditional -->
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile__admin-badge" v-else>
        Common User
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong>{{ followers }}
      </div>
      <div class="user-profile__favourite">
        {{ favouritedTwoot }}
      </div>
      <!--  @add-twoot="addNewTwootToTwoots", capture the data from child component-->
      <CreateTwootPanel @add-twoot="addNewTwootToTwoots"/>
    </div>
    <div class="user-profile__twoots-wrapper">
      <!-- v-for loop and data passing -->
      <!-- @favourite="toggleFavourite" capture the emit date -->
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";
import CreateTwootPanel from "./CreatTwootPanel";
export default {
  name: "UserProfile",
  components: { TwootItem,CreateTwootPanel },
  data() {
    return {
      followers: 0,
      favouritedTwoot: "",
      user: {
        id: 1,
        username: "_DavidDong",
        firstName: "David",
        lastName: "Dong",
        email: "daviddong201612@gmail.com",
        isAdmin: false,
        twoots: [
          { id: 1, content: "Learing Vue.js" },
          { id: 2, content: "Learing NodeJs" },
          { id: 3, content: "Learing Java" },
          { id: 4, content: "Learing MySQL" }
        ]
      }
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },

  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      this.favouritedTwoot = `My favorite twoot's id is ${id}`;
    },
    addNewTwootToTwoots(newTwoot){
      this.user.twoots.unshift(
        {id:this.user.twoots.length+1, content:newTwoot})
    }

  },
  mounted() {
    this.followUser();
  }
};
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
  background: red;
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
  padding: 0 20px;
}
.user-profile__twoots-wrapper {
  display: grid;
  grid-gap: 10px;
}


h1 {
  margin: 0;
}
</style>
