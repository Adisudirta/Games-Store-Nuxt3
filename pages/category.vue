<script setup lang="ts">
const isError = ref(false);
const games = ref([]);
const errorMsg = ref("");

async function search(val: string) {
  const { data } = await useAsyncData("game", () =>
    $fetch(
      `https://free-to-play-games-database.p.rapidapi.com/api/games?category=${val}`,
      {
        headers: {
          "x-rapidapi-host": "free-to-play-games-database.p.rapidapi.com",
          "x-rapidapi-key":
            "ea3093d472msh6c90fc42a3fa0f0p105117jsn5a10aadec347",
        },
      }
    ).catch((error) => {
      isError.value = true;
      errorMsg.value = error;
    })
  );

  games.value = data.value;
}
</script>

<template>
  <NuxtLayout name="common">
    <SearchApp
      @search="search"
      title="Search your game base on Category"
      egMessage="eg: mmorpg, shooter, pvp, mmofps"
    />
    <MessageApp
      v-if="games.length === 0 && isError === false"
      srcImg="../assets/images/doge.png"
      altImg="doge-icon"
      msg="Your search results will be displayed here"
      emoticon="&#128270;"
    />
    <MessageApp
      v-if="isError"
      srcImg="../assets/images/error-icon.png"
      altImg="error-icon"
      :msg="errorMsg"
      emoticon="&#128546;"
    />
    <section
      v-if="games.length !== 0 && isError === false"
      class="w-full min-h-screen bg-gray-900 px-10 py-10 sm:px-20 md:px-28"
    >
      <h2 class="font-bold text-gray-400 mb-10">
        Here are your search results &#10024;
      </h2>
      <div
        class="grid gap-5 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <CardApp
          v-for="game in games"
          :key="game.id"
          :srcImg="game.thumbnail"
          :title="game.title"
          :desc="game.short_description"
          :category="game.genre"
          :platform="game.platform"
        />
      </div>
    </section>
  </NuxtLayout>
</template>
