<script>
  import { onMount } from "svelte";
  import "../../app.css";
  import axios from "axios";
  let token;
  let refreshToken;
  const baseUrl = "https://accounts.spotify.com/";
  const client_id = "4ad9ba662edb47edafe174ac302e81b6";
  const client_secret = "97a4ef41a9334308a6f746ff7ca1fdec";
  const redirect_uri = "http://localhost:5173/";
  const state = "Test";
  const scopes = [
    "streaming",
    "user-read-private",
    "user-modify-playback-state",
    "user-read-email",
    "user-top-read",
    "playlist-modify-public",
    "playlist-modify-private",
    "playlist-read-private",
  ];

  onMount(async () => {
    token = localStorage.getItem("access_token");
    refreshToken = localStorage.getItem("refresh_token");

    if (!token) {
      goto("/");
    }

    console.log("Made it " + token);
  });

  function getData() {
    console.log(token);
    axios
      .get("https://api.spotify.com/v1/me/top/artists?limit=10", {
        headers: {
          Authorization: `Bearer ${token}`,
          "Content-Type": "application/x-www-form-urlencoded",
        },
      })
      .then((res) => console.log(res))
      .catch((err) => console.log(err));
  }
</script>

<div>Hi this is a dashboard</div>
<button on:click={getData} class="btn">Get Data</button>

<style>
</style>
