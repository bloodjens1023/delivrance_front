<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import HeaderAttenteSuperAdmin from "../../../Components/HeaderAttenteSuperAdmin.svelte";
  import Pub from "../../../Components/Pub.svelte";
  import { goto } from "$app/navigation";
  import FooterAttenteSuperAdmin from "../../../Components/FooterAttenteSuperAdmin.svelte";
  import Chargement from "../../../Components/Chargement.svelte";

  let loads = true;
  setTimeout(() => {
    loads = false;
  }, 1000);

  let donne = {
    un: "1",
    deux: "2",
    trois: "3",
    quatre: "4",
    cinq: "5",
    six: "6",
  };
  let pub = false;
  let success = false;
  let error = false;
  let loading = false;
  const handleSubmit = async (event) => {
    event.preventDefault();
    let val =
      donne.un +
      "" +
      donne.deux +
      "" +
      donne.trois +
      "" +
      donne.quatre +
      "" +
      "" +
      donne.cinq +
      "" +
      "" +
      donne.six +
      "";
    let formdata = new FormData();
    formdata.append("code", val);

    let response;
    response = await fetch("https://bloodjens.pythonanywhere.com/api_code/", {
      method: "POST",
      body: formdata,
    });

    loading = true;

    const data = await response.json();
    const message = data.message;
    console.log(message);

    if (message) {
      loading = false;
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
    } else {
      console.error("Erreur");
      error = true;
      loading = false;
      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };

  let post = [];
  const getPosts = async () => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/api_codeVerif/"
    );

    const data = await res.json();
    const filter = data.data;
    return filter;
  };
  onMount(async () => {
    try {
      let id = sessionStorage.getItem("admin");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        post = await getPosts();
        post = post[0]["code"].split("");
        donne.un = post[0];
        donne.deux = post[1];
        donne.trois = post[2];
        donne.quatre = post[3];
        donne.cinq = post[4];
        donne.six = post[5];
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<title>Generateur</title>
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
    Code Géneré et enregistrer avec succès
  </div>
{/if}

<HeaderAttenteSuperAdmin gener="active" />
<br /><br /><br />
{#if loads}
  <Chargement />
{/if}
{#if !loads}
  <center>
    <div class="radio-inputs">
      <label class="radio">
        <input
          type="radio"
          name="radio"
          checked
          on:click={() => {
            pub = false;
          }}
        />
        <span class="name">Génerer un code de verification </span>
      </label>
      <label class="radio">
        <input
          type="radio"
          name="radio"
          on:click={() => {
            pub = true;
          }}
        />
        <span class="name">Créer un publication</span>
      </label>
    </div>
  </center>

  {#if !pub}
    <center
      style="height: 60vh; display: flex; align-items: center; justify-content: center;"
    >
      <form class="form">
        <div class="title">OTP</div>
        <div class="title">Code de Vérification</div>
        <p class="message">
          Generer le code de Vérification pour les chef d'arrondissement
        </p>
        <div class="inputs">
          <input
            id="input1"
            type="text"
            maxlength="1"
            bind:value={donne.un}
            disabled
          />
          <input
            id="input2"
            type="text"
            maxlength="1"
            bind:value={donne.deux}
            disabled
          />
          <input
            id="input3"
            type="text"
            maxlength="1"
            bind:value={donne.trois}
            disabled
          />
          <input
            id="input4"
            type="text"
            maxlength="1"
            bind:value={donne.quatre}
            disabled
          />
          <input
            id="input4"
            type="text"
            maxlength="1"
            bind:value={donne.cinq}
            disabled
          />
          <input
            id="input4"
            type="text"
            maxlength="1"
            bind:value={donne.six}
            disabled
          />
        </div>
        <br />
        <div style="display: flex; gap:30px">
          <button
            class="btn btn-outline-dark"
            on:click={() => {
              // verifier = true;

              donne.un = Math.floor(Math.random() * 9).toString();
              donne.deux = Math.floor(Math.random() * 9).toString();
              donne.trois = Math.floor(Math.random() * 9).toString();
              donne.quatre = Math.floor(Math.random() * 9).toString();
              donne.cinq = Math.floor(Math.random() * 9).toString();
              donne.six = Math.floor(Math.random() * 9).toString();
            }}>Regenerer</button
          >
          <button class="btn btn-success" on:click={handleSubmit}
            >Enregistrer</button
          >
        </div>
      </form>
      <br />
      <br />
    </center>
  {/if}

  {#if pub}
    <br /><br /><br />
    <center>
      <Pub />
    </center>
  {/if}
{/if}
<br /><br /><br />
<FooterAttenteSuperAdmin />

<style>
  @import url("$lib/bootstrap.min.css");
  .radio-inputs {
    position: relative;
    display: flex;
    flex-wrap: wrap;
    border-radius: 0.5rem;
    background-color: #eee;
    box-sizing: border-box;
    box-shadow: 0 0 0px 1px rgba(0, 0, 0, 0.06);
    padding: 0.25rem;
    width: 600px;
    font-size: 14px;
  }

  .radio-inputs .radio {
    flex: 1 1 auto;
    text-align: center;
  }

  .radio-inputs .radio input {
    display: none;
  }

  .radio-inputs .radio .name {
    display: flex;
    cursor: pointer;
    align-items: center;
    justify-content: center;
    border-radius: 0.5rem;
    border: none;
    padding: 0.5rem 0;
    color: rgba(51, 65, 85, 1);
    transition: all 0.15s ease-in-out;
  }

  .radio-inputs .radio input:checked + .name {
    background-color: #fff;
    font-weight: 600;
  }
  .form {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: space-around;
    width: 400px;
    background-color: rgb(255, 255, 255);
    border: 3px solid black;
    border-radius: 12px;
    padding: 20px;
  }

  .title {
    font-size: 20px;
    font-weight: bold;
    color: black;
  }

  .message {
    color: #a3a3a3;
    font-size: 14px;
    margin-top: 4px;
    text-align: center;
  }

  .inputs {
    margin-top: 10px;
  }

  .inputs input {
    width: 32px;
    height: 32px;
    text-align: center;
    border: none;
    border-bottom: 1.5px solid #d2d2d2;
    margin: 0 10px;
  }

  .inputs input:focus {
    border-bottom: 1.5px solid royalblue;
    outline: none;
  }
</style>
