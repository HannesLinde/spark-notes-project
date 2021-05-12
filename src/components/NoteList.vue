<template>
  <div class="border-b-4 border-indigo-100">
    <div
      v-for="note in notes"
      :key="note.id"
      class="grid grid-cols-5 space-x-2 border-b"
    >
      <router-link
        class="block py-4 col-span-4"
        :to="{ name: 'Note', params: { id: note.id } }"
      >
        <div>
          <h3 class="text-xs text-indigo-500">
            {{ note.collection }}
          </h3>
          <h2 class="text-lg font-medium">{{ note.title }}</h2>
          <span class="text-gray-400 text-xs"> by {{ userId }}</span>
          <p class="overflow-hidden text-overflow-ellipsis h-12">
            {{ note.content }}
          </p>
          <span class="font-semibold"> ...</span>
          <p class="text-sm text-gray-400 mt-2 text-right">
            {{ new Date(note.createdAt).toLocaleDateString() }}
          </p>
        </div>
      </router-link>
      <div class="flex place-items-start pt-4">
        <button
          @click="$emit('request:delete')"
          class="text-red-600 border border-gray-500 boder border-solid rounded-md px-2 py-1 hover:bg-red-600 hover:text-indigo-800"
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