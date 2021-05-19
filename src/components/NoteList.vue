<template>
  <div class="border-b-4 border-indigo-100">
    <div
      v-for="note in notes"
      :key="note.id"
      class="grid grid-cols-5 space-x-2 border-b bg-yellow-200 rounded-lg px-2 my-1 border-gray-600"
    >
      <router-link
        class="block py-4 col-span-4"
        :to="{ name: 'Note', params: { id: note.id } }"
      >
        <div>
          <div class="flex justify-between">
            <h3 class="text-xs tracking-wide text-indigo-800">
              #{{ note.collection }}
            </h3>
            <div class="text-xs tracking-wide text-gray-700 mt-2 text-right">
              {{ new Date(note.createdAt).toLocaleDateString() }}
            </div>
          </div>
          <h2 class="text-lg font-medium">{{ note.title }}</h2>
          <span class="text-gray-700 text-xs">
            by {{ $store.state.user.id }}</span
          >
          <p class="overflow-hidden text-overflow-ellipsis h-12 mt-4">
            {{ note.content }}
          </p>
          <span class="font-semibold"> ...</span>
        </div>
      </router-link>
      <div class="flex justify-end place-items-start py-4">
        <button
          @click="$emit('request:delete')"
          class="text-sm border border-gray-500 boder border-solid rounded-md px-1 hover:bg-indigo-300"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      showConfirmation: false,
    };
  },
  props: {
    notes: {
      type: [],
    },
  },
});
</script>

<style scoped>
</style>