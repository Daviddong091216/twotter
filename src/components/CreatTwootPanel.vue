<template>
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
</template>

<script>
export default {
  name: "CreateTwootPanel",
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" }
      ]
    };
  },
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        //   emit data from child component
        this.$emit("add-twoot", this.newTwootContent);
        // clean the textarea
        this.newTwootContent = "";
      }
    }
  }
};
</script>

<style scoped>
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
