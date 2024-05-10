<script>
  import { Motion } from "svelte-motion";
  import Header from "../../../Components/Header.svelte";
  import Footer from "../../../Components/Footer.svelte";
  import { fade } from "svelte/transition";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import ChargementConnect from "../../../Components/ChargementConnect.svelte";

  let visible = false;

  onMount(() => {
    visible = true;
  });

  /**
   * @type {HTMLAnchorElement}
   */
  let link;
  let info = "";
  let identifiant = "";
  let password = "";
  let success = false;
  let error = false;
  let loading = false;

  async function handleSubmit() {
    loading = true;
    const response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_connexion/",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ identifiant, password }),
      }
    );

    const data = await response.json();
    const message = data.message;
    info = data.info;
    console.log(message);

    if (message) {
      console.log("utilisateur inserer");
      // Redirection ou autre action après la création réussie

      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      loading = false;

      if (identifiant == "admin") {
        sessionStorage.setItem("admin", identifiant);
        goto("/SuperAdmin/HomeSuperAdmin");
      } else {
        sessionStorage.setItem("identifiant", identifiant);
        goto("/Utilisateur/Attente");
      }
    } else {
      console.error("Erreur");
      error = true;

      setTimeout(() => {
        error = false;
      }, 1000);
      loading = false;
    }
  }
</script>

{#if success}
  <div
    class="alert alert-success"
    role="alert"
    style="position: fixed; bottom: 0; left: 20px"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-check-circle-fill"
      viewBox="0 0 16 16"
      style="margin-right: 10px;"
    >
      <path
        d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0m-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z"
      />
    </svg>
    Utilisateur connecté
  </div>
{/if}

{#if error}
  <div
    class="alert alert-danger d-flex align-items-center"
    role="alert"
    style="position: fixed; bottom: 0; left: 20px"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-exclamation-circle-fill"
      viewBox="0 0 16 16"
      style="margin-right: 10px;"
    >
      <path
        d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M8 4a.905.905 0 0 0-.9.995l.35 3.507a.552.552 0 0 0 1.1 0l.35-3.507A.905.905 0 0 0 8 4m.002 6a1 1 0 1 0 0 2 1 1 0 0 0 0-2"
      />
    </svg>
    <div>{info}</div>
  </div>
{/if}
<div>
  <Header />
  <br />
  <br />
  <center
    transition:fade={{ duration: 500 }}
    style="height: 600px; display: flex; flex-direction: column; align-items: center; justify-content: center"
  >
    <h1 class="mb-5" style="font-weight: bold;">Connectez-vous</h1>
    <form>
      <div class="forms">
        <div class="input-container">
          <input required id="input" type="text" bind:value={identifiant} />
          <label class="label" for="input">Identifiant</label>
          <div class="underline"></div>
        </div>
        <div class="input-container">
          <input required id="input" type="password" bind:value={password} />
          <label class="label" for="input">Mots de passe</label>
          <div class="underline"></div>
        </div>
        <div class="input-container">
          {#if loading}
            <ChargementConnect />
          {/if}
        </div>
        {#if !loading}
          <div class="input-container">
            <Motion let:motion whileHover={{ rotate: "3deg" }}>
              <input
                type="button"
                value="Se connecter"
                class="button"
                use:motion
                on:click={() => {
                  handleSubmit();
                }}
              />
            </Motion>
          </div>
        {/if}
      </div>
    </form>
    <br />
    <br />
  </center>
  <div style="position: static;">
    <Footer />
  </div>
</div>

<style>
  @import url("$lib/bootstrap.min.css");
  .input-container {
    position: relative;
    margin: 50px auto;
    width: 350px;
  }

  .input-container input[type="text"],
  .input-container input[type="password"] {
    font-size: 20px;
    width: 100%;
    border: none;
    border-bottom: 2px solid #000000;
    padding: 5px 0;
    background-color: transparent;
    outline: none;
  }

  .input-container .label {
    position: absolute;
    top: 0;
    left: 0;
    color: #000000;
    transition: all 0.3s ease;
    pointer-events: none;
  }

  .input-container input[type="text"]:focus ~ .label,
  .input-container input[type="text"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    font-weight: bold;
    color: #333;
  }

  .input-container input[type="password"]:focus ~ .label,
  .input-container input[type="password"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    color: #333;

    font-weight: bold;
  }

  .input-container .underline {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    width: 100%;
    background-color: #333;
    transform: scaleX(0);
    transition: all 0.3s ease;
  }

  .input-container input[type="text"]:focus ~ .underline,
  .input-container input[type="text"]:valid ~ .underline,
  .input-container input[type="password"]:focus ~ .underline,
  .input-container input[type="password"]:valid ~ .underline {
    transform: scaleX(1);
  }

  .button {
    width: 100%;
    height: auto;
    padding: 10px 0;
    font-size: 1.3em;
    font-weight: bold;
    background-color: white;
    border: 4px solid #5f6468;
    color: #5f6468;
    border-radius: 3px 3px 20px 3px;
    transition: 0.2s all;
  }
  .button:hover {
    background-color: #5f6468;
    color: white;
  }
</style>
