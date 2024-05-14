<script>
  // @ts-nocheck
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import Modal from "./Modal.svelte";
  import ChargementConnect from "./ChargementConnect.svelte";
  import Chargement from "./Chargement.svelte";

  let success = false;
  let error = false;
  $: actuel = [];
  let showModal = false;
  let suppr = "";
  const getPosts = async (user) => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/supprimerChef/" + user
    );

    const data = await res.json();
    const filter = data;
    if (filter) {
      users = [];
      fetchUtilisateur();
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      return true;
    } else {
      return false;
    }
  };

  let users = [];
  let filtrer = users;
  async function fetchUtilisateur() {
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/afficheArrondissementChefTout/"
      );
      const data = await response.json();
      users = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      filtrer = users;
      totalPages = Math.ceil(filtrer.length / itemsPerPage);
      actuel = getCurrentPageData();
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateur:", error);
    }
  }

  let recherche = "";
  onMount(() => {
    fetchUtilisateur();
  });
  function search() {
    goToPage(1);
    filtrer = [];
    for (const i of users) {
      if (i["nom"].toLowerCase().includes(recherche.toLowerCase())) {
        filtrer.push(i);
      }
    }
    totalPages = Math.ceil(filtrer.length / itemsPerPage);
    actuel = getCurrentPageData();
  }

  let itemsPerPage = 4; // Nombre d'éléments par page
  let currentPage = 1; // Page actuelle

  // Calcul du nombre total de pages
  let totalPages = Math.ceil(filtrer.length / itemsPerPage);

  // Fonction pour passer à la page suivante ou précédente
  function goToPage(page) {
    if (page >= 1 && page <= totalPages) {
      currentPage = page;
    }
  }

  // Obtention des éléments pour la page actuelle
  function getCurrentPageData() {
    const startIndex = (currentPage - 1) * itemsPerPage;
    const endIndex = startIndex + itemsPerPage;
    actuel = filtrer.slice(startIndex, endIndex);
    console.log(actuel);
    return filtrer.slice(startIndex, endIndex);
  }

  // Déplacement à la page suivante
  function next() {
    if (currentPage < totalPages) {
      currentPage++;
      goToPage(currentPage);
      actuel = getCurrentPageData();
    }
  }

  // Déplacement à la page précédente
  function prev() {
    if (currentPage > 1) {
      actuel = getCurrentPageData();
      currentPage--;
      goToPage(currentPage);
      actuel = getCurrentPageData();
    }
  }
</script>

{#if success}
  <div
    class="alert alert-success"
    role="alert"
    style="position: fixed; bottom: 0; left: 20px; display: flex; gap:12px;"
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
    Chef supprimer avec success
  </div>
{/if}

{#if error}
  <div
    class="alert alert-danger d-flex align-items-center"
    role="alert"
    style="position: fixed; bottom: 0; right: 20px;  display: flex; gap:12px;"
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
    <div>Erreur de connexion</div>
  </div>
{/if}

{#if showModal}
  <Modal bind:showModal>
    <h2 slot="header">⚠ Attention !</h2>
    <hr />
    <br />
    <ol class="definition-list">
      voulez-vous vraiment supprimer cet Utilisateur ?
    </ol>
    <hr />
    <div style="display: flex; gap:10px">
      <button
        class="btn btn-dark"
        on:click={() => {
          showModal = false;
          getPosts(suppr);
        }}>oui</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = false;
        }}>non</button
      >
    </div>
  </Modal>
{/if}
{#await fetchUtilisateur()}
  <center><Chargement /></center>
{:then ds}
  <!-- code de recherche -->
  <center>
    <form class="form">
      <label for="search">
        <input
          class="input"
          type="text"
          required=""
          bind:value={recherche}
          on:input={() => {
            search();
          }}
          placeholder="Rechercher un Email ..."
          id="search"
        />
        <div class="fancy-bg"></div>
        <div class="search">
          <svg
            viewBox="0 0 24 24"
            aria-hidden="true"
            class="r-14j79pv r-4qtqp9 r-yyyyoo r-1xvli5t r-dnmrzs r-4wgw6l r-f727ji r-bnwqim r-1plcrui r-lrvibr"
          >
            <g>
              <path
                d="M21.53 20.47l-3.66-3.66C19.195 15.24 20 13.214 20 11c0-4.97-4.03-9-9-9s-9 4.03-9 9 4.03 9 9 9c2.215 0 4.24-.804 5.808-2.13l3.66 3.66c.147.146.34.22.53.22s.385-.073.53-.22c.295-.293.295-.767.002-1.06zM3.5 11c0-4.135 3.365-7.5 7.5-7.5s7.5 3.365 7.5 7.5-3.365 7.5-7.5 7.5-7.5-3.365-7.5-7.5z"
              ></path>
            </g>
          </svg>
        </div>
      </label>
    </form>
    <br /><br />
  </center>
  <!-- fin code de recherche -->
  <center>
    <h2>Liste des chef d'arrondissements</h2>
  </center>
  <br />

  <table>
    <thead>
      <tr>
        <th scope="col">Nom</th>
        <th scope="col">prenom</th>
        <th scope="col">email</th>
        <th scope="col">CNI</th>
        <th scope="col">Tel</th>
        <th scope="col">Dirigant</th>

        <th scope="col">Supprimer</th>
      </tr>
    </thead>

    <tbody>
      {#each actuel as user}
        <tr>
          <td data-label="Nom">{user["nom"]}</td>
          <td data-label="Prénom">{user["prenom"]}</td>

          <td data-label="Email">{user["email"]}</td>
          <td data-label="CNI">{user["cni"]}</td>
          <td data-label="Tel">{user["tel"]}</td>
          <td data-label="Dirigeant"
            >{user["dirige"]} / {user["district"]} / {user["region"]}</td
          >
          <td>
            <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
              <button
                class="btn btn-danger"
                use:motion
                on:click={() => {
                  showModal = true;
                  suppr = user["email"];
                }}
                ><svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-trash3"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5M11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47M8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5"
                  />
                </svg></button
              ></Motion
            ></td
          >
        </tr>
      {/each}
    </tbody>
  </table>
  <br />
  {#if actuel.length > 0}
    <div
      style="display: flex; align-items: center; justify-content: center; gap: 100px;"
    >
      <button on:click={prev} class="btn btn-dark btn-sm">prev</button>
      <div
        style="height: 100%; display: flex; align-items: center; justify-content: center; margin-top: 10px;"
      >
        <p>Page {currentPage} sur {totalPages}</p>
      </div>
      <button on:click={next} class="btn btn-dark btn-sm">next</button>
    </div>
  {/if}

  <br />
  <br />
  {#if users.length == 0}
    <p>Aucun Chef d'arrondissement</p>
  {/if}
{/await}

<style>
  table {
    border: 1px solid #ccc;
    border-collapse: collapse;
    margin: 0;
    padding: 0;
    width: 90%;
    table-layout: fixed;
  }

  table tr {
    background-color: #f8f8f8;
    border: 1px solid #ddd;
    padding: 0.35em;
  }

  table th,
  table td {
    text-align: center;
    padding: 10px;
    word-wrap: break-word;
    font-size: 0.9rem;
  }

  table th {
    font-size: 0.85em;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    width: 100%;
  }

  @media screen and (max-width: 600px) {
    table {
      border: 0;
    }

    table thead {
      border: none;
      clip: rect(0 0 0 0);
      height: 1px;
      margin: -1px;
      overflow: hidden;
      padding: 0;
      position: absolute;
      width: 1px;
    }

    table tr {
      border-bottom: 3px solid #ddd;
      display: block;
      margin-bottom: 0.625em;
    }

    table td {
      border-bottom: 1px solid #ddd;
      display: block;
      font-size: 0.8em;
      text-align: right;
      word-wrap: break-word;
    }

    table td::before {
      /*
    * aria-label has no advantage, it won't be read inside a table
    content: attr(aria-label);
    */
      content: attr(data-label);
      float: left;
      font-weight: bold;
      text-transform: uppercase;
    }

    table td:last-child {
      border-bottom: 0;
    }
  }
  /* debut css de code de rejcercje */
  .form {
    --input-text-color: #fff;
    --input-bg-color: #283542;
    --focus-input-bg-color: transparent;
    --text-color: #949faa;
    --active-color: #1b9bee;
    --width-of-input: 600px;
    --inline-padding-of-input: 1.2em;
    --gap: 0.9rem;
  }
  /* form style */
  .form {
    font-size: 0.9rem;
    display: flex;
    gap: 0.5rem;
    align-items: center;
    width: var(--width-of-input);
    position: relative;
    isolation: isolate;
  }
  /* a fancy bg for showing background and border when focus. */
  .fancy-bg {
    position: absolute;
    width: 100%;
    inset: 0;
    background: var(--input-bg-color);
    border-radius: 30px;
    height: 100%;
    z-index: -1;
    pointer-events: none;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  }
  /* label styling */
  label {
    width: 100%;
    padding: 0.8em;
    height: 40px;
    padding-inline: var(--inline-padding-of-input);
    display: flex;
    align-items: center;
  }

  .search {
    position: absolute;
  }
  /* styling search-icon */
  .search {
    fill: var(--text-color);
    left: var(--inline-padding-of-input);
  }
  /* svg -- size */
  svg {
    width: 17px;
    display: block;
  }
  /* styling of close button */

  /* styling of input */
  .input {
    color: var(--input-text-color);
    width: 100%;
    margin-inline: min(2em, calc(var(--inline-padding-of-input) + var(--gap)));
    background: none;
    border: none;
  }

  .input:focus {
    outline: none;
    color: black;
  }

  .input::placeholder {
    color: var(--text-color);
  }
  /* input background change in focus */
  .input:focus ~ .fancy-bg {
    border: 1px solid var(--active-color);
    background: var(--focus-input-bg-color);
  }
  /* search icon color change in focus */
  .input:focus ~ .search {
    fill: var(--active-color);
  }
  /* showing close button when typing */

  /* this is for the default background in input,when selecting autofill options -- you can remove this code if you do not want to override the browser style.  */
  input:-webkit-autofill,
  input:-webkit-autofill:hover,
  input:-webkit-autofill:focus,
  input:-webkit-autofill:active {
    -webkit-transition: "color 9999s ease-out, background-color 9999s ease-out";
    -webkit-transition-delay: 9999s;
  }
  /* fin code de recherche */
</style>
