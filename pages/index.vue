<script setup lang="ts">
const isError = ref(false);

const { data: games } = await useAsyncData("game", () =>
  $fetch("https://free-to-play-games-database.p.rapidapi.com/api/games", {
    headers: {
      "x-rapidapi-host": "free-to-play-games-database.p.rapidapi.com",
      "x-rapidapi-key": "ea3093d472msh6c90fc42a3fa0f0p105117jsn5a10aadec347",
    },
  }).catch(() => {
    isError.value = true;
  })
);
</script>

<template>
  <NuxtLayout name="common">
    <MessageApp
      v-if="isError"
      srcImg="../assets/images/error-icon.png"
      altImg="error-icon"
      msg="Something Error!"
      emoticon="&#128546;"
    />
    <section
      class="w-full min-h-screen bg-gray-900 px-10 py-10 sm:px-20 md:px-28"
    >
      <h2 class="font-bold text-gray-400 mb-10">
        Maybe you like the game below &#129321;
      </h2>
      <div
        class="grid gap-5 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <CardApp
          v-for="game in games.slice(0, 8)"
          :key="game.id"
          :srcImg="game.thumbnail"
          :title="game.title"
          :desc="game.short_description"
          :category="game.genre"
          :platform="game.platform"
        />
      </div>
    </section>
    <section class="bg-gray-900 px-10 py-4 sm:px-20 md:px-56">
      <div
        class="rounded-lg p-10 bg-[url('../assets/images/motivated-vergil.jpg')] bg-cover bg-center text-center flex flex-col gap-5"
      >
        <h2 class="text-3xl font-bold text-gray-100">
          Nothing is interesting? &#129300;
        </h2>
        <p class="text-lg font-bold text-gray-400">
          Still haven't found the game you want? Let's find your fun game based
          on:
        </p>
        <div class="text-gray-100 items-center flex justify-center gap-3">
          <button class="rounded font-bold px-2 py-1 bg-sky-700">
            <NuxtLink to="/category">By Category</NuxtLink>
          </button>

          <span class="text-gray-400">Or</span>

          <button class="rounded font-bold px-2 py-1 bg-sky-500">
            <NuxtLink to="/platform">By Platform</NuxtLink>
          </button>
        </div>
      </div>
    </section>
  </NuxtLayout>
</template>
