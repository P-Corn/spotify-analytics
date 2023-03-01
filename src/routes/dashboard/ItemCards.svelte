<script>
  export let data = [];
  export let type = "";

  function goToLink(link) {
    window.location = link;
  }
</script>

<div class="flex items-stretch flex-wrap">
  {#each data as item, i (item.href)}
    <div class="basis-1/1 sm:basis-1/2 lg:basis-1/3 xl:basis-1/4">
      <div class="card card-compact bg-gray-800 shadow-xl m-5">
        <figure class="image-container">
          <img
            on:click={() => goToLink(item.external_urls.spotify)}
            on:keydown={() => goToLink(item.external_urls.spotify)}
            class="item-image"
            src={type === "artists"
              ? item.images[0].url
              : item.album.images[0].url}
            alt="Artist or Song"
          />
        </figure>
        <div class="card-body">
          {#if type === "artists"}
            <p>{item.genres.slice(0, 2).join(" | ")}</p>
          {:else if type === "tracks"}
            <p>
              {item.artists
                .map((a) => a.name)
                .slice(0, 2)
                .join(" | ")}
            </p>
          {/if}
          <h2 class="card-title text-xl">
            <span>{i + 1}. </span>{item.name.length > 19
              ? item.name.slice(0, 21) + "..."
              : item.name}
          </h2>
        </div>
      </div>
    </div>
  {/each}
</div>

<style>
  .card-title {
  }
  .card-body p {
    height: 20px;
  }
  .item-image {
    max-width: 100%;
    width: auto;
    cursor: pointer;
  }
  .image-container {
    aspect-ratio: 1/1;
  }
</style>
