<script>
  // @ts-nocheck

  import { Motion } from "svelte-motion";
  import Header from "../../../Components/Header.svelte";
  import Footer from "../../../Components/Footer.svelte";
  import ChargementConnect from "../../../Components/ChargementConnect.svelte";
  import { blur } from "svelte/transition";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";

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
  let email = "";
  let password = "";
  let tel = "";
  let arrond = 1;
  let region = 1;
  let district = 1;
  let success = false;
  let error = false;
  let loading = false;

  async function handleSubmit() {
    console.log(arrond);
    loading = true;
    const response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_inscription/",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ identifiant, email, tel, arrond, password }),
      }
    );

    const data = await response.json();
    const message = data.message;
    info = data.info;
    console.log(message);

    if (message) {
      console.log("utilisateur inserer");
      // Redirection ou autre action après la création réussie
      loading = false;
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      sessionStorage.setItem("identifiant", identifiant);
      goto("/Utilisateur/Step");
    } else {
      console.error("Erreur");
      error = true;
      loading = false;
      setTimeout(() => {
        error = false;
      }, 1000);
    }
  }

  let arrondissements = [];
  let districts = [];
  let regions = [];
  let id = [];
  async function fetchArrondissements() {
    arrondissements = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/get_arrond_district/" + district
      );
      const data = await response.json();
      arrondissements = data.data;
      arrond = "";
    } catch (error) {
      console.error(
        "Erreur lors de la récupération des arrondissements:",
        error
      );
    }
  }
  async function fetchregion() {
    arrondissements = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/get_region/"
      );
      const data = await response.json();
      regions = data.data;
      arrond = "";
    } catch (error) {
      console.error(
        "Erreur lors de la récupération des arrondissements:",
        error
      );
    }
  }

  async function fetchDistrict() {
    districts = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/get_district/" + region
      );
      const data = await response.json();
      districts = data.data;
      arrond = "";
    } catch (error) {
      console.error(
        "Erreur lors de la récupération des arrondissements:",
        error
      );
    }
  }

  onMount(() => {
    let users = sessionStorage.getItem("identifiant");
    fetchregion();
  });
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
    Utilisateur inscrit avec success
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
  <center transition:blur={{ duration: 500 }}>
    <h1 class="mb-5" style="font-weight: bold;">Inscrivez-vous</h1>

    <form action="Post">
      <div class="forms">
        <div class="input-container">
          <input required id="input" type="text" bind:value={identifiant} />
          <label class="label" for="input">Identifiant</label>
          <div class="underline"></div>
        </div>
        <div class="input-container">
          <input required id="input" type="email" bind:value={email} />
          <label class="label" for="input">email</label>
          <div class="underline"></div>
        </div>
        <div class="input-container">
          <input
            required
            id="input"
            type="tel"
            bind:value={tel}
            maxlength="10"
          />
          <label class="label" for="input">Tel</label>
          <div class="underline"></div>
        </div>
        <div class="input-container">
          <div class="select">
            <select
              on:change={(e) => {
                region = e.target.value;
                if (e.target.value != "") {
                  fetchDistrict();
                } else {
                  arrondissements = [];
                  districts = [];
                }
              }}
            >
              <option value="">Votre région</option>
              {#each regions as a}
                <option value={a["id"]}>{a["nom"]} </option>
              {/each}
            </select>
          </div>
        </div>

        <div class="input-container">
          <div class="select">
            <select
              on:change={(e) => {
                district = e.target.value;
                if (e.target.value != "") {
                  fetchArrondissements();
                } else {
                  arrondissements = [];
                }
              }}
            >
              <option value="">Votre district</option>
              {#each districts as a}
                <option value={a["id"]}>{a["nom"]} </option>
              {/each}
            </select>
          </div>
        </div>
        <div class="input-container">
          <div class="select">
            <select
              on:change={(e) => {
                arrond = e.target.value;
              }}
            >
              <option value="">Votre arrondissement</option>
              {#each arrondissements as a}
                <option value={a["id"]}>{a["nom"]} </option>
              {/each}
            </select>
          </div>
        </div>

        <div class="input-container">
          <input
            required
            id="input"
            type="password"
            bind:value={password}
            minlength="8"
          />
          <label class="label" for="input">Mots de passe</label>
          <div class="underline"></div>
        </div>
        {#if loading}
          <ChargementConnect />
        {/if}
        {#if !loading}
          <div class="input-container">
            <Motion let:motion whileHover={{ rotate: "3deg" }}>
              <input
                type="button"
                on:click={() => {
                  handleSubmit();
                }}
                value="S'inscrire"
                class="button"
                use:motion
              />
            </Motion>
          </div>
        {/if}
      </div>
    </form>

    <br />
    <br />
  </center>
  <footer style="position: static">
    <Footer />
  </footer>
</div>

<style>
  @import url("$lib/bootstrap.min.css");
  .input-container {
    position: relative;
    margin: 50px auto;
    width: 350px;
  }
  select {
    /* Reset Select */
    appearance: none;
    outline: 10px red;

    box-shadow: none;
    /* Personalize */
    flex: 1;
    padding: 0 1em;
    color: #000000;
    background-color: var(--darkgray);
    background-image: none;
    cursor: pointer;
    border: 3px solid #000000;
  }
  /* Remove IE arrow */
  select::-ms-expand {
    display: none;
  }
  /* Custom Select wrapper */
  .select {
    position: relative;
    display: flex;
    width: 20em;
    height: 3em;
    border-radius: 0.25em;
    overflow: hidden;
  }
  /* Arrow */
  .select::after {
    content: "\25BC";
    position: absolute;
    top: 0;
    right: 0;
    padding-top: 3%;
    width: 15%;
    height: 100%;
    background-color: #000000;
    transition: 0.25s all ease;
    pointer-events: none;
    border: 3px solid black;
    color: white;
  }

  .input-container input[type="text"],
  .input-container input[type="email"],
  .input-container input[type="tel"],
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
  .input-container input[type="email"]:focus ~ .label,
  .input-container input[type="email"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    color: #333;

    font-weight: bold;
  }
  .input-container input[type="password"]:focus ~ .label,
  .input-container input[type="password"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    color: #333;

    font-weight: bold;
  }
  .input-container input[type="tel"]:focus ~ .label,
  .input-container input[type="tel"]:valid ~ .label {
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
  .input-container input[type="password"]:valid ~ .underline,
  .input-container input[type="tel"]:focus ~ .underline,
  .input-container input[type="tel"]:valid ~ .underline,
  .input-container input[type="email"]:focus ~ .underline,
  .input-container input[type="email"]:valid ~ .underline {
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
