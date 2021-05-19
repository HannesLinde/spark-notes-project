<template>
  <div>
    <div
      class="flex justify-between space-x-4 fixed bg-white max-w-screen-sm mx-auto left-0 right-0 top-0 p-2"
    >
      <div class="flex flex-start space-x-4">
        <router-link
          :to="{ name: 'CreateNote' }"
          class="rounded-md border border-gray-500 bg-indigo-300 border-solid px-2 py-1 hover:bg-indigo-100 hover:text-gray-800"
          >Take a note</router-link
        >
        <router-link
          :to="{ name: 'Dashboard' }"
          class="rounded-md border border-gray-500 bg-indigo-300 border-solid px-2 py-1 hover:bg-indigo-100 hover:text-gray-800"
          v-if="showDashboardButton"
          >Dashboard</router-link
        >
      </div>
      <div class="flex flex-end space-x-4">
        <input
          v-if="showSearchBar"
          type="text"
          placeholder="search"
          @input="$emit('update:keyword', $event.target.value)"
          class="text-gray-500 text-center w-24 border-t border-b border-gray-500 border-solid px-2 py-1 focus:border-indigo-300 focus:outline-none"
        />
        <button
          @click="logout"
          class="px-2 hover:bg-indigo-100 hover:text-gray-800 hover:border hover:rounded-md"
        >
          Logout
        </button>
      </div>
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
    showSearchBar: {
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