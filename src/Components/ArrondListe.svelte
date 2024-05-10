<script>
  // @ts-nocheck
  import { goto } from "$app/navigation";
  import { onDestroy, onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import Modal from "./Modal.svelte";

  let showModal = false;
  let suppr = "";
  let modif = false;
  let ajout = false;
  let success = false;
  let error = false;
  let region = "";
  let regions = [];
  let codes = "";
  let ids = "";
  let noms = "";
  $: actuel = [];

  const getPosts = async (user) => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/supprimerArrondissement/" + user
    );

    const data = await res.json();
    const filter = data;
    if (filter) {
      users = [];
      fetchUtilisateur();
      suppr = "";
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
        "https://bloodjens.pythonanywhere.com/afficheArrondissement/"
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

  function modifier(id, nom, code, region_id) {
    ids = id;
    noms = nom;
    codes = code;
    region = region_id;
    modif = true;
    ajout = false;
    scrollToElement("modif");
  }

  // code ajout district
  const handleSubmit = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    formdata.append("nom", noms);
    formdata.append("code", codes);
    formdata.append("district", region);

    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_insertion_arrondissement/",
      {
        method: "POST",
        body: formdata,
      }
    );

    const data = await response.json();
    const message = data.message;
    console.log(message);
    fetchUtilisateur();
    ajout = false;
    window.scrollTo({
      top: 0,
      behavior: "smooth", // Pour un défilement en douceur
    });
    if (message) {
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      noms = "";

      // sessionStorage.setItem("identifiant", identifiant);
    } else {
      console.error("Erreur");
      error = true;

      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };

  // code mise a jour arrondissement
  const handleSubmit2 = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    formdata.append("nom", noms);
    formdata.append("code", codes);
    formdata.append("district", region);

    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/updateArrondissement/" + ids,
      {
        method: "POST",
        body: formdata,
      }
    );

    const data = await response.json();
    const message = data.message;
    console.log(message);
    fetchUtilisateur();
    window.scrollTo({
      top: 0,
      behavior: "smooth", // Pour un défilement en douceur
    });
    if (message) {
      noms = "";
      modif = false;

      // sessionStorage.setItem("identifiant", identifiant);
    } else {
      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };
  async function fetchregion() {
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/afficheDistrict/"
      );
      const data = await response.json();
      regions = data.data;
    } catch (error) {
      console.error(
        "Erreur lors de la récupération des arrondissements:",
        error
      );
    }
  }

  let recherche = "";
  onMount(() => {
    fetchUtilisateur();
    fetchregion();
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

  function scrollToElement(id) {
    var element = document.getElementById(id);
    if (element) {
      element.scrollIntoView({
        behavior: "smooth", // Animation de défilement fluide
        block: "start", // Défilement vers le haut de l'élément
      });
    } else {
      console.warn("L'élément avec l'ID spécifié n'a pas été trouvé.");
    }
  }

  // let u = setInterval(() => {
  //   fetchUtilisateur();
  // }, 500);

  // onDestroy(() => {
  //   clearInterval(u);
  // });
</script>

{#if showModal}
  <Modal bind:showModal>
    <h2 slot="header">⚠ Attention !</h2>
    <hr />
    <br />
    <ol class="definition-list">
      voulez-vous vraiment supprimer cet district ?
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

<center style="display: flex; gap: 30px;">
  <h2>Liste des arrondissements à Madagascar</h2>
  <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
    <button
      class="btn btn-success"
      use:motion
      on:click={() => {
        ajout = true;
        modif = false;
        noms = "";
        codes = "";
        region = "";
        scrollToElement("modif");
      }}
      ><svg
        xmlns="http://www.w3.org/2000/svg"
        width="30"
        height="30"
        fill="currentColor"
        class="bi bi-patch-plus-fill"
        viewBox="0 0 16 16"
      >
        <path
          d="M10.067.87a2.89 2.89 0 0 0-4.134 0l-.622.638-.89-.011a2.89 2.89 0 0 0-2.924 2.924l.01.89-.636.622a2.89 2.89 0 0 0 0 4.134l.637.622-.011.89a2.89 2.89 0 0 0 2.924 2.924l.89-.01.622.636a2.89 2.89 0 0 0 4.134 0l.622-.637.89.011a2.89 2.89 0 0 0 2.924-2.924l-.01-.89.636-.622a2.89 2.89 0 0 0 0-4.134l-.637-.622.011-.89a2.89 2.89 0 0 0-2.924-2.924l-.89.01zM8.5 6v1.5H10a.5.5 0 0 1 0 1H8.5V10a.5.5 0 0 1-1 0V8.5H6a.5.5 0 0 1 0-1h1.5V6a.5.5 0 0 1 1 0"
        />
      </svg></button
    ></Motion
  >
</center>
<br />
<br />
<!-- code de recherche -->
<center>
  <input
    id="input"
    name="text"
    placeholder="Rechercher un district"
    type="search"
    bind:value={recherche}
    on:input={() => {
      search();
    }}
  />
</center>
<br />
<br />
<!-- fin code de recherche -->

<table>
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Nom</th>
      <th scope="col">district associée</th>
      <th scope="col">code</th>

      <th scope="col">Modifier</th>
      <th scope="col">Supprimer</th>
    </tr>
  </thead>

  <tbody>
    {#each actuel as user, i}
      <tr>
        <td data-label="Account">{i + 1}</td>
        <td data-label="Account">{user["nom"]}</td>
        <td data-label="Account">{user["district"]}</td>
        <td data-label="Account">{user["code"]}</td>

        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-warning"
              use:motion
              on:click={() => {
                modifier(
                  user["id"],
                  user["nom"],
                  user["code"],
                  user["district_id"]
                );
              }}
              ><svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-pencil"
                viewBox="0 0 16 16"
              >
                <path
                  d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325"
                />
              </svg></button
            ></Motion
          ></td
        >
        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-danger"
              use:motion
              on:click={() => {
                suppr = user["id"];
                showModal = true;
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
{#if actuel.length == 0}
  <p>Aucun District</p>
{/if}

<br />
<br />

<div id="modif">
  <!-- modification District -->
  {#if modif}
    <center>
      <form class="form-control" action="">
        <p class="title">Modifier le Arrondissement</p>
        <div class="input-field">
          <input
            required=""
            class="input"
            type="text"
            bind:value={noms}
            on:change={(e) => {
              noms = e.target.value;
            }}
          />
          <label class="label" for="input">Nom de l'arrondissement</label>
        </div>
        <div class="input-container">
          <div class="select">
            <select
              on:select={(e) => {
                region = e.target.value;
              }}
              bind:value={region}
            >
              <option value="">District associer</option>
              {#each regions as a}
                <option value={a["id"]}>{a["nom"]} </option>
              {/each}
            </select>
          </div>
        </div>
        <div class="input-field">
          <input
            required=""
            class="input"
            type="text"
            bind:value={codes}
            on:change={(e) => {
              codes = e.target.value;
            }}
          />
          <label class="label" for="input">code associer au district</label>
        </div>
        <br />
        <button class="btn btn-dark" on:click={handleSubmit2}>modifier</button>
        <button
          class="btn btn-danger"
          on:click={() => {
            window.scrollTo({
              top: 0,
              behavior: "smooth", // Pour un défilement en douceur
            });
            modif = false;
          }}>annuler</button
        >
      </form>
    </center>
  {/if}

  <!-- fin -->
</div>

<div id="ajout">
  <!-- ajout arrondissement -->
  {#if ajout}
    <center>
      <form class="form-control" action="">
        <p class="title">Ajout d'un district</p>
        <div class="input-field">
          <input
            required=""
            class="input"
            type="text"
            bind:value={noms}
            on:change={(e) => {
              noms = e.target.value;
            }}
          />
          <label class="label" for="input">Nom de la district</label>
        </div>
        <div class="input-container">
          <div class="select">
            <select
              on:change={(e) => {
                region = e.target.value;
              }}
            >
              <option value="">District associer</option>
              {#each regions as a}
                <option value={a["id"]}>{a["nom"]} </option>
              {/each}
            </select>
          </div>
        </div>
        <div class="input-field">
          <input
            required=""
            class="input"
            type="text"
            bind:value={codes}
            on:change={(e) => {
              codes = e.target.value;
            }}
          />
          <label class="label" for="input">code associer au district</label>
        </div>
        <br />
        <button class="btn btn-dark" on:click={handleSubmit}>ajouter</button>
        <button
          class="btn btn-danger"
          on:click={() => {
            window.scrollTo({
              top: 0,
              behavior: "smooth", // Pour un défilement en douceur
            });
            ajout = false;
          }}>annuler</button
        >
      </form>
    </center>
  {/if}
</div>

<!-- fin -->

<style>
  /* css arrondissement */
  .form-control {
    margin: 20px;
    background-color: #ffffff;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
    width: 400px;
    display: flex;
    justify-content: center;
    flex-direction: column;
    gap: 10px;
    padding: 25px;
    border-radius: 8px;
  }
  .title {
    font-size: 28px;
    font-weight: 800;
  }
  .input-field {
    position: relative;
    width: 100%;
  }

  .input {
    margin-top: 15px;
    width: 100%;
    outline: none;
    border-radius: 8px;
    height: 45px;
    border: 1.5px solid #ecedec;
    background: transparent;
    padding-left: 10px;
  }
  .input:focus {
    border: 1.5px solid #000000;
  }
  .input-field .label {
    position: absolute;
    top: 25px;
    left: 15px;
    color: #ccc;
    transition: all 0.3s ease;
    pointer-events: none;
    z-index: 2;
  }
  .input-field .input:focus ~ .label,
  .input-field .input:valid ~ .label {
    top: 5px;
    left: 5px;
    font-size: 12px;
    color: #000000;
    background-color: #ffffff;
    padding-left: 5px;
    padding-right: 5px;
  }
  /* fin css arondissement */

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

  #input {
    max-width: 800px;
    width: 800px;
    background-color: #f5f5f5;
    color: #242424;
    padding: 0.15rem 0.5rem;
    min-height: 40px;
    border-radius: 4px;
    outline: none;
    border: none;
    line-height: 1.15;
    box-shadow: 0px 10px 20px -18px;
  }

  #input:focus {
    border-bottom: 2px solid #5b5fc7;
    border-radius: 4px 4px 2px 2px;
  }

  #input:hover {
    outline: 1px solid lightgrey;
  }

  .input-container {
    position: relative;
    margin: 10px auto;
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
    border: 2px solid #000000;
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
</style>
