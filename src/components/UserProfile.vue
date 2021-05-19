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
      <!-- @submit.prevent="createNewTwoot", event listener -->
      <!-- :class="{ 'exceed': newTwootCharacterCount > 180 } dynamic class, class conditional -->
      <form
        class="user-profile__create-twoot"
        @submit.prevent="createNewTwoot"
        :class="{ exceed: newTwootCharacterCount > 180 }"
      >
        <label for="newTwoot"
          ><strong>New Twoot</strong>({{ newTwootCharacterCount }}/180)</label
        >
        <!-- v-model form data binding -->
        <textarea id="newTwoot" rows="4" v-model="newTwootContent" />
        <div class="user-profile__create-twoot-type">
          <label for="newTwootType"><strong>Type:</strong></label>
          <select id="newTwootType" v-model="selectedTwootType">
            <option
              v-for="option in twootTypes"
              :key="option.id"
              :value="option.value"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <button class="button">
          Twoot!
        </button>
      </form>
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
export default {
  name: "UserProfile",
  components: { TwootItem },
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" }
      ],
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
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      this.favouritedTwoot = `My favorite twoot's id is ${id}`;
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent
        });
        // clean the textarea
        this.newTwootContent = "";
      }
    }
  },
  mounted() {
    this.followUser();
  }
};
</script>

<style  scoped>
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
.user-profile__create-twoot {
  padding-top: 20px;
  display: flex;
  flex-direction: column;
}
.user-profile__create-twoot-type {
  margin-top: 20px;
}

.button {
  background: darkgreen;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  margin-top: 20px;
  padding: 0 20px;
}
.exceed {
  color: red;
  border-color: red;
}
h1 {
  margin: 0;
}
</style>
