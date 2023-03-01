<script>
  import { onMount } from "svelte";
  import "../../app.css";
  import axios from "axios";

  let token;
  let topArtists = [];
  let topTracks = [];

  function getUserTopArtists() {
    axios
      .get("https://api.spotify.com/v1/me/top/artists?limit=10", {
        headers: {
          Authorization: `Bearer ${token}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
      })
      .then((res) => (topArtists = res.data.items))
      .catch((err) => console.log(err));
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
      .catch((err) => console.log(err));
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

<div class="container mx-auto">
  <div class="flex flex-wrap">
    {#each topArtists as artist}
      <div class="basis-1/2">
        <div class="card lg:card-side shadow-xl m-3">
          <figure>
            <img class="item-image" src={artist.images[0].url} alt="Album" />
          </figure>
          <div class="card-body">
            <h2 class="card-title text-xl">{artist.name}</h2>
            <p>{artist.genres?.join(" | ")}</p>
            <div class="card-actions justify-end">
              <button class="btn btn-primary">Listen</button>
            </div>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>

<button on:click={getUserTopArtists} class="btn">Get Artists</button>
<button on:click={getUserTopTracks} class="btn">Get Tracks</button>

<style>
  .item-image {
    max-width: 360px;
    height: 360px;
  }
</style>
