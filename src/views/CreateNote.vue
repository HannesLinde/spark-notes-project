<template>
  <div>
    <h1>Take a note</h1>
    <form @submit.prevent="submit">
      <div>
        <label for="title" class="sr-only">Title</label>
        <input type="text" name="title" placeholder="Title" v-model="title" />
      </div>
      <div>
        <label for="content" class="sr-only">Content</label>
        <textarea placeholder="Content" v-model="content" />
      </div>
      <div>
        <button type="submit">Create</button>
      </div>
    </form>
    <router-link :to="{ name: 'Dashboard' }"
      >Back to notes dashboard</router-link
    >
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

const now = new Date();

export default Vue.extend({
  data() {
    return {
      title: "",
      content: "",
    };
  },
  methods: {
    async submit() {
      console.log("Submitttti!");
      const response = await axios.post("http://localhost:3000/notes", {
        title: this.title,
        content: this.content,
        createdAt: now,
        createdBy: this.$store.state.user.email,
      });
      console.log(response);
      this.$router.push("/");
    },
  },
});
</script>

<style scoped>
</style>