<script>
  // @ts-nocheck
  import { goto } from "$app/navigation";
  import { onDestroy, onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import Modal from "./Modal.svelte";
  import ChargementConnect from "./ChargementConnect.svelte";
  function insererEspace(str) {
    return str.replace(/\d{3}(?=\d)/g, "$& ");
  }
  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  let showModal = false;
  let suppr = "";
  let modif = false;

  let n = "";
  let p = "";
  let a = "";
  let nc = "";
  let ph = "";

  $: actuel = [];

  const getPosts = async (user) => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/supprimerDistrict/" + user
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
        "https://bloodjens.pythonanywhere.com/affiche_cni_tous/"
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

  function info(nom, prenom, adresse, num, photo) {
    n = nom;
    p = prenom;
    a = adresse;
    nc = num;
    ph = photo;

    modif = true;
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

{#if !modif}
  <center style="display: flex; gap: 30px;">
    <h2>Liste des CNI livré à chaque arrondissement</h2>
  </center>
  <br />
  <br />
  <!-- code de recherche -->
  <center>
    <input
      id="input"
      name="text"
      placeholder="Rechercher un CNI"
      type="search"
      bind:value={recherche}
      on:input={() => {
        search();
      }}
    />
  </center>
  <br />
  <br />
{/if}
<!-- fin code de recherche -->
{#await fetchUtilisateur()}
  <br />
  <ChargementConnect />
{:then ds}
  {#if !modif}
    <table>
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Nom</th>
          <th scope="col">Prenom</th>
          <th scope="col">Numéro</th>

          <th scope="col">Info</th>
        </tr>
      </thead>

      <tbody>
        {#each actuel as user, i}
          <tr>
            <td data-label="ID">{i + 1}</td>
            <td data-label="Nom">{user["nom"]}</td>
            <td data-label="Prenom">{user["prenom"]}</td>
            <td data-label="Numéro">{insererEspace(user["numCNI"])}</td>

            <td>
              <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
                <button
                  class="btn btn-dark"
                  use:motion
                  on:click={() => {
                    info(
                      user["nom"],
                      user["prenom"],
                      user["adresse"],
                      user["numCNI"],
                      user["photo"]
                    );
                  }}
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="currentColor"
                    class="bi bi-info-circle-fill"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M8 16A8 8 0 1 0 8 0a8 8 0 0 0 0 16m.93-9.412-1 4.705c-.07.34.029.533.304.533.194 0 .487-.07.686-.246l-.088.416c-.287.346-.92.598-1.465.598-.703 0-1.002-.422-.808-1.319l.738-3.468c.064-.293.006-.399-.287-.47l-.451-.081.082-.381 2.29-.287zM8 5.5a1 1 0 1 1 0-2 1 1 0 0 1 0 2"
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
  {/if}

  <br />
  <br />
  {#if actuel.length == 0}
    <p>Aucun District</p>
  {/if}

  <br />
  <br />
{/await}

<div id="modif">
  <!-- modification District -->
  {#if modif}
    <div>
      <form class="form-control" action="">
        <center><p class="title">Information sur le CNI</p></center>
        <center><img src={filter(ph)} alt="" width="300px" /></center>
        <br /><br />
        <div style="margin-left: 100px;">
          <p style="font-family: monospace; font-size: 20px;">nom : {n}</p>
          <p style="font-family: monospace;  font-size: 20px;">prenom : {p}</p>
          <p style="font-family: monospace;  font-size: 20px;">adresse : {a}</p>
          <p style="font-family: monospace; font-size: 20px;">
            numero CNI : {nc}
          </p>
        </div>
        <br /><br />
        <button
          class="btn btn-outline-dark"
          on:click={() => {
            window.scrollTo({
              top: 0,
              behavior: "smooth", // Pour un défilement en douceur
            });
            modif = false;
          }}>Fermer</button
        >
      </form>
    </div>
  {/if}

  <!-- fin -->
</div>

<style>
  /* css arrondissement */
  .form-control {
    /* margin: 20px; */
    background-color: #ffffff;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
    width: 800px;
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
