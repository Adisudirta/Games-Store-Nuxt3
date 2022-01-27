<script setup lang="ts">
const searchInput = ref("");

const props = defineProps({
  title: String,
  egMessage: String,
});

const emit = defineEmits(["search"]);

function search() {
  emit("search", searchInput.value);
  searchInput.value = "";
}
</script>

<template>
  <div
    class="flex flex-col gap-3 w-full bg-gray-900 px-10 py-4 sm:px-20 md:px-28"
  >
    <h2 class="text-center text-gray-300">{{ title }}</h2>
    <form @submit.prevent="search" class="flex justify-center gap-4">
      <input v-model="searchInput" class="rounded px-3 w-72" type="text" />
      <button
        type="submit"
        class="rounded bg-sky-900 text-gray-300 font-bold px-3"
      >
        Search &#128269;
      </button>
    </form>
    <p class="text-gray-500 text-center">{{ egMessage }}</p>
  </div>
</template>
