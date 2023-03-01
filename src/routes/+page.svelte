<script>
  import { onMount } from "svelte";
  import "../app.css";
  import axios from "axios";
  import { goto } from "$app/navigation";
  import { PUBLIC_CLIENT_SECRET, PUBLIC_CLIENT_ID } from "$env/static/public";

  const baseUrl = "https://accounts.spotify.com/";
  const client_id = PUBLIC_CLIENT_ID;
  const client_secret = PUBLIC_CLIENT_SECRET;
  const redirect_uri = "https://thunderous-pithivier-141fb5.netlify.app/";
  const state = "Testing";
  const scopes = ["user-top-read"];

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
        .post(`${baseUrl}api/token`, new URLSearchParams(data).toString(), {
          "Content-Type": "application/x-www-form-urlencoded",
          Authorization: "Basic " + btoa(client_id + ":" + client_secret),
        })
        .then((res) => {
          const { access_token, refresh_token } = res.data;
          localStorage.setItem("access_token", access_token);
          localStorage.setItem("refresh_token", refresh_token);
          goto("/dashboard");
        })
        .catch((err) => console.log(err));
    }

    if (error) {
      console.log(error);
    }
  });

  function logIn() {
    window.location = url;
  }
</script>

<div class="text-center login-container m-auto">
  <h1>Your Spotify Analytics</h1>
  <p class="mb-5 mt-2">
    Hi! This is a simple app that lets you view your top artists and tracks on
    Spotify. The app is still a work in progress, more features will be added.
  </p>
  <button class="btn" on:click={logIn}>Log in</button>
</div>

<style>
  h1 {
    font-size: 2rem;
    font-weight: 600;
  }
  p {
    max-width: 600px;
  }
  .login-container {
    height: 100vh;
    display: flex;
    padding: 0 1rem;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>
