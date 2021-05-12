<template>
  <div>
    <div class="grid grid-cols-5 space-x-2 border-b">
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
        <DeleteButton />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    note: {
      type: Object,
    },
  },
});
</script>

<style scoped>
</style>