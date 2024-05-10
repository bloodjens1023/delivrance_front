<script>
  import { goto } from "$app/navigation";
  import { Motion } from "svelte-motion";
  export let simple = false;
  let u = false;
  let d = false;
  let t = false;
  let q = false;
  let c = false;
  let note = 0;
  async function envoyer() {
    if (u) {
      note = 5;
    } else if (d) {
      note = 4;
    } else if (t) {
      note = 3;
    } else if (q) {
      note = 2;
    } else if (c) {
      note = 1;
    } else {
      note = 0;
    }
    let id = sessionStorage.getItem("identifiant");
    let formdata = new FormData();
    formdata.append("note", note.toString());
    const response = await fetch(
      "https://bloodjens.pythonanywhere.com/ajouter_retour/" + id,
      {
        method: "POST",
        body: formdata,
      }
    );
    goto("/Utilisateur/Attente");
  }
</script>

<div class="contenu">
  <center><h1>votre niveau de satisfaction sur notre application</h1></center>
  <br />
  <center>
    <div class="rating">
      <input
        type="checkbox"
        id="star-1"
        name="star-radio"
        value="star-1"
        bind:checked={u}
      />
      <label for="star-1">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
          ><path
            pathLength="360"
            d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
          ></path></svg
        >
      </label>
      <input
        type="checkbox"
        id="star-2"
        name="star-radio"
        value="star-1"
        bind:checked={d}
      />
      <label for="star-2">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
          ><path
            pathLength="360"
            d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
          ></path></svg
        >
      </label>
      <input
        type="checkbox"
        id="star-3"
        name="star-radio"
        value="star-1"
        bind:checked={t}
      />
      <label for="star-3">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
          ><path
            pathLength="360"
            d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
          ></path></svg
        >
      </label>
      <input
        type="checkbox"
        id="star-4"
        name="star-radio"
        value="star-1"
        bind:checked={q}
      />
      <label for="star-4">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
          ><path
            pathLength="360"
            d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
          ></path></svg
        >
      </label>
      <input
        type="checkbox"
        id="star-5"
        name="star-radio"
        value="star-1"
        bind:checked={c}
      />
      <label for="star-5">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
          ><path
            pathLength="360"
            d="M12,17.27L18.18,21L16.54,13.97L22,9.24L14.81,8.62L12,2L9.19,8.62L2,9.24L7.45,13.97L5.82,21L12,17.27Z"
          ></path></svg
        >
      </label>
    </div>
  </center>
  <br /><br />
  {#if u}
    <center>
      <h1>🤩</h1>
    </center>
  {:else if d}
    <center>
      <h1>🥰</h1>
    </center>
  {:else if t}
    <center>
      <h1>😘</h1>
    </center>
  {:else if q}
    <center>
      <h1>🙄</h1>
    </center>
  {:else if c}
    <center>
      <h1>😞</h1>
    </center>
  {:else}
    <center>
      <h1>😬</h1>
    </center>
  {/if}
  <br />
  <center>
    <button class="btn btn-outline-dark" on:click={envoyer}>envoyer</button>
  </center>
  <br />
</div>
<br />
{#if !simple}
  <Motion let:motion whileHover={{ scale: 1.2 }} whileTap={{ scale: 1.1 }}>
    <a class="btn btn-success" href="/Utilisateur/Step" use:motion
      >envoyer un nouvelle demande</a
    >
  </Motion>
{/if}

<style>
  .contenu {
    border: 3px solid black;
    width: 90%;
    padding: 40px;
  }
  h1 {
    font-size: 2em;
    font-variant: small-caps;
    font-weight: bolder;
    text-decoration: underline;
  }

  .rating {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: row-reverse;
    gap: 0.3rem;
    --stroke: #666;
    --fill: #ffc73a;
  }

  .rating input {
    appearance: unset;
  }

  .rating label {
    cursor: pointer;
  }

  .rating svg {
    width: 2rem;
    height: 2rem;
    overflow: visible;
    fill: transparent;
    stroke: var(--stroke);
    stroke-linejoin: bevel;
    stroke-dasharray: 12;
    animation: idle 4s linear infinite;
    transition:
      stroke 0.2s,
      fill 0.5s;
  }

  @keyframes idle {
    from {
      stroke-dashoffset: 24;
    }
  }

  .rating label:hover svg {
    stroke: var(--fill);
  }

  .rating input:checked ~ label svg {
    transition: 0s;
    animation:
      idle 4s linear infinite,
      yippee 0.75s backwards;
    fill: var(--fill);
    stroke: var(--fill);
    stroke-opacity: 0;
    stroke-dasharray: 0;
    stroke-linejoin: miter;
    stroke-width: 8px;
  }

  @keyframes yippee {
    0% {
      transform: scale(1);
      fill: var(--fill);
      fill-opacity: 0;
      stroke-opacity: 1;
      stroke: var(--stroke);
      stroke-dasharray: 10;
      stroke-width: 1px;
      stroke-linejoin: bevel;
    }

    30% {
      transform: scale(0);
      fill: var(--fill);
      fill-opacity: 0;
      stroke-opacity: 1;
      stroke: var(--stroke);
      stroke-dasharray: 10;
      stroke-width: 1px;
      stroke-linejoin: bevel;
    }

    30.1% {
      stroke: var(--fill);
      stroke-dasharray: 0;
      stroke-linejoin: miter;
      stroke-width: 8px;
    }

    60% {
      transform: scale(1.2);
      fill: var(--fill);
    }
  }
</style>
