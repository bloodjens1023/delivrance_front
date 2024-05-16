<script>
  // @ts-nocheck

  import { goto } from "$app/navigation";

  // @ts-nocheck

  import { Motion } from "svelte-motion";

  export let contenu =
    "Vous avez reçu une notification sur votre demande de délivrance de CNI du 10/20/2023";
</script>

{#if contenu["lue"]}
  <Motion
    let:motion
    whileHover={{ boxShadow: "3px 3px 3px black", scale: 1.02 }}
    whileTap={{ scale: 1 }}
  >
    <button
      use:motion
      class="bg-dark text-white not"
      on:click={() => {
        goto(contenu["lien"]);
      }}
    >
      <div style="display: flex; flex-direction: column;">
        <p>{contenu["description"]}</p>
        <div>
          <ul>
            <li>
              <p style="text-decoration: underline; width: 140px;">
                Il y a {contenu["date"]}
              </p>
            </li>
          </ul>
        </div>
      </div>
      <div style="margin-right: 50px;">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          height="20"
          fill="currentColor"
          class="bi bi-bell-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="M8 16a2 2 0 0 0 2-2H6a2 2 0 0 0 2 2m.995-14.901a1 1 0 1 0-1.99 0A5 5 0 0 0 3 6c0 1.098-.5 6-2 7h14c-1.5-1-2-5.902-2-7 0-2.42-1.72-4.44-4.005-4.901"
          />
        </svg>
      </div>
    </button>
  </Motion>
{/if}

<style>
  .not {
    width: 100%;
    height: 100%;
    padding: 20px 20px;
    border: none;
    cursor: pointer;
    display: flex;
    justify-content: space-around;
    gap: 10px;
    background-color: black;
    border-radius: 10px;
  }
  @media only screen and (max-width: 768px) {
    .not {
      font-size: 0.7em;
      padding: 10px 10px;
    }
  }
</style>
