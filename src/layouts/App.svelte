<script lang="ts">
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";

  let term = "";
  let photos: {
    id: string;
    alt_description: string;
    urls: { raw: string };
  }[] = [];

  async function getResponse() {
    const response = await fetch(
      `https://api.unsplash.com/search/photos?page=1&per_page=12&query=${
        term || "office"
      }&client_id=kWL_X3-94HU4yg-bYcBtXAg_qDBfRe99zpAGc82OxfI`
    );
    const data = await response.json();
    photos = data.results;
  }

  onMount(() => {
    getResponse();
    console.log(photos);
  });

  async function handleSearch() {
    if (!term) return;
    await getResponse();
    term = "";
  }
</script>

<main
  class="bg-[conic-gradient(at_right,_var(--tw-gradient-stops))] from-indigo-200 via-slate-600 to-indigo-200"
>
  <div class="container mx-auto">
    <header class="flex flex-col items-center justify-center">
      <h1 class="py-8 text-4xl font-bold uppercase text-white">
        Image Gallery
      </h1>
      <div class="flex gap-1">
        <input
          type="text"
          class="w-full rounded border border-cyan-500 bg-gray-50 px-5 py-2.5 focus:outline-none focus:ring-2 focus:ring-cyan-500"
          bind:value={term}
        />
        <button
          class="rounded border border-cyan-500 bg-cyan-500 px-5 py-2.5 text-white focus:outline-none focus:ring-2 focus:ring-cyan-500 active:bg-cyan-600"
          on:click={handleSearch}>Search</button
        >
      </div>
    </header>
    <div class="grid gap-4 px-8 py-16 md:grid-cols-2 xl:grid-cols-3">
      {#each photos as photo, i (photo.id)}
        <img
          src={photo.urls.raw + "&fit=crop&h=360&w=640"}
          alt={photo.alt_description}
          class="rounded shadow-xl duration-300 hover:scale-105"
          in:fly={{ y: 200, duration: 2000, delay: i * 200 }}
          out:fade
        />
      {/each}
    </div>
  </div>
</main>
