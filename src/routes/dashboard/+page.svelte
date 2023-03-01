<script>
  import { onMount } from "svelte";
  import "../../app.css";
  import axios from "axios";
  import ItemCards from "./ItemCards.svelte";
  import { goto } from "$app/navigation";

  export let drawer = null;
  let token;
  let topArtists = [];
  let topTracks = [];
  let currentTab = "artists";
  $: topTitle = `TOP 10 ${currentTab.toUpperCase()}`;

  function getUserTopArtists() {
    axios
      .get("https://api.spotify.com/v1/me/top/artists?limit=10", {
        headers: {
          Authorization: `Bearer ${token}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
      })
      .then((res) => (topArtists = res.data.items))
      .catch((err) => goto("/"));
  }

  function getUserTopTracks() {
    axios
      .get("https://api.spotify.com/v1/me/top/tracks?limit=10", {
        headers: {
          Authorization: `Bearer ${token}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
      })
      .then((res) => (topTracks = res.data.items))
      .catch((err) => goto("/"));
  }

  onMount(async () => {
    token = localStorage.getItem("access_token");

    if (!token) {
      goto("/");
    }

    getUserTopArtists();
    getUserTopTracks();
  });
</script>

<div class="drawer">
  <input
    bind:this={drawer}
    id="my-drawer-3"
    type="checkbox"
    class="drawer-toggle"
  />
  <div class="drawer-content flex flex-col">
    <!-- Navbar -->
    <div class="w-full navbar bg-base-300">
      <div class="flex-none lg:hidden">
        <label for="my-drawer-3" class="btn btn-square btn-ghost">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            class="inline-block w-6 h-6 stroke-current"
            ><path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16M4 18h16"
            /></svg
          >
        </label>
      </div>
      <div class="flex-1 px-2 mx-2">{topTitle}</div>
      <div class="flex-none hidden lg:block">
        <ul class="menu menu-horizontal">
          <!-- Navbar menu content here -->
          <li
            on:click={() => (currentTab = "artists")}
            on:keydown={() => (currentTab = "artists")}
            class="cursor-pointer btn"
          >
            Top Artists
          </li>
          <li
            on:click={() => (currentTab = "tracks")}
            on:keydown={() => (currentTab = "tracks")}
            class="ml-5 cursor-pointer btn"
          >
            Top Tracks
          </li>
        </ul>
      </div>
    </div>
    <div class="container mx-auto">
      {#if currentTab === "artists"}
        <ItemCards data={topArtists} type="artists" />
      {:else}
        <ItemCards data={topTracks} type="tracks" />
      {/if}
    </div>
  </div>
  <div class="drawer-side">
    <label for="my-drawer-3" class="drawer-overlay" />
    <ul class="menu p-4 w-80 bg-base-100">
      <li
        on:click={() => {
          currentTab = "artists";
          drawer.checked = false;
        }}
        on:keydown={() => {
          currentTab = "artists";
          drawer.checked = false;
        }}
        class="cursor-pointer btn mb-3 drawer-button"
      >
        Top Artists
      </li>
      <li
        on:click={() => {
          currentTab = "tracks";
          drawer.checked = false;
        }}
        on:keydown={() => {
          currentTab = "tracks";
          drawer.checked = false;
        }}
        class="cursor-pointer btn"
      >
        Top Tracks
      </li>
    </ul>
  </div>
</div>

<style>
</style>
