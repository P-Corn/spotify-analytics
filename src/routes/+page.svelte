<script>
  import { onMount } from "svelte";
  import "../app.css";
  import axios from "axios";

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

  const url =
    `${baseUrl}authorize?` +
    new URLSearchParams({
      response_type: "code",
      client_id,
      scope: scopes.join(" "),
      redirect_uri,
      state,
    });

  onMount(() => {
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get("code");
    const error = urlParams.get("error");

    const data = {
      grant_type: "authorization_code",
      code,
      redirect_uri,
      client_id,
      client_secret,
      code_verifier: state,
    };

    if (code) {
      axios
        .post(`${baseUrl}api/token`, new URLSearchParams(data).toString())
        .then((res) => {
          console.log(res);
        })
        .catch((err) => console.log(err));
    }

    if (error) {
      console.log(error);
      alert("There was an error during the authentication, please try again.");
    }
  });

  function logIn() {
    window.location = url;
  }
</script>

<div>
  <h1>View your Spotify Analytics</h1>
  <p>First, log in to Spotify. Then, you can view your Spotify Analytics.</p>
  <button class="btn" on:click={logIn}>Log in</button>
</div>

<style>
  h1 {
    font-size: 2rem;
    font-weight: 600;
  }
</style>
