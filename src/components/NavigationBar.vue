<template>
  <div>
    <div
      class="flex justify-center space-x-4 fixed bg-white max-w-screen-sm mx-auto left-0 right-0 top-0 p-2"
    >
      <router-link
        :to="{ name: 'CreateNote' }"
        class="text-gray-500 rounded-md border border-indigo-500 border-solid px-2 py-1 hover:bg-indigo-300 hover:text-gray-800"
        v-if="showCreateButton"
        >Take new note</router-link
      >
      <router-link
        :to="{ name: 'Dashboard' }"
        class="text-gray-500 rounded-md border border-indigo-500 border-solid px-2 py-1 hover:bg-indigo-300 hover:text-gray-800"
        v-if="showDashboardButton"
        >Back to notes dashboard</router-link
      >
      <input
        type="text"
        placeholder="search"
        @input="$emit('update:keyword', $event.target.value)"
      />

      <button @click="logout" class="text-sm hover:text-indigo-500">
        Logout
      </button>
    </div>
    <div class="pt-12"></div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    showDashboardButton: {
      type: Boolean,
      default: true,
    },
    showCreateButton: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    logout() {
      this.$store.commit("CLEAR");
      this.$router.push({ name: "Login" });
    },
  },
});
</script>

<style scoped>
</style>