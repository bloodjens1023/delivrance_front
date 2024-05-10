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
  let ids = "";
  let noms = "";
  let prenoms = "";
  let adresses = "";
  let photos = "";
  let motifs = "";
  let codes = "";
  $: actuel = [];

  function filter(a) {
    return (
      "https://www.pythonanywhere.com/user/bloodjens/files/home/bloodjens/" + a
    );
  }
  const getPosts = async (user) => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/updateRdv/" + user
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
        "https://bloodjens.pythonanywhere.com/afficheRdvTous/"
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

  function modifier(id, nom, prenom, adresse, photo, type, code) {
    ids = id;
    noms = nom;
    prenoms = prenom;
    adresses = adresse;
    photos = filter(photo);
    motifs = type;
    codes = code;
    modif = true;
    ajout = false;
    scrollToElement("modif");
  }

  // code mise a jour arrondissement
  const handleSubmit2 = async (event) => {
    event.preventDefault();

    let response;
    response = fetch("https://bloodjens.pythonanywhere.com/updateRdv/" + ids);

    console.log(response);
    // const data = await response.json();
    // const message = data.data;
    // console.log(message);
    fetchUtilisateur();
    let message = true;
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
        console.log("erreur");
      }, 1000);
    }
  };

  let recherche = "";
  onMount(() => {
    fetchUtilisateur();
  });
  function search() {
    goToPage(1);
    filtrer = [];
    for (const i of users) {
      if (i["code"].toString().includes(recherche)) {
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

  let u = setInterval(() => {
    fetchUtilisateur();
  }, 500);

  onDestroy(() => {
    clearInterval(u);
  });
</script>

{#if showModal}
  <Modal bind:showModal>
    <h2 slot="header">Information</h2>
    <hr />
    <br />
    <ol class="definition-list">
      voulez-vous vraiment rendre cet rendez-vous ?
    </ol>
    <hr />
    <div style="display: flex; gap:10px">
      <button
        class="btn btn-success"
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
  <h2>Liste des CNI non récuperer</h2>
</center>
<br />
<br />
<!-- code de recherche -->
<center>
  <input
    id="input"
    name="text"
    placeholder="Rechercher un arrondissement"
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
      <th scope="col">Code de verification</th>
      <th scope="col">date de recuperation</th>
      <th scope="col">heure de recuperation</th>
      <th scope="col">recuperer</th>
      <th scope="col">info</th>
    </tr>
  </thead>

  <tbody>
    {#each actuel as user, i}
      <tr>
        <td data-label="Account">{i + 1}</td>
        <td data-label="Account">{user["code"]}</td>
        <td data-label="Account">{user["date_fin"]}</td>
        <td data-label="Account">{user["heure"]}</td>

        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-success"
              use:motion
              on:click={() => {
                suppr = user["id"];
                showModal = true;
              }}
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-receipt"
                viewBox="0 0 16 16"
              >
                <path
                  d="M1.92.506a.5.5 0 0 1 .434.14L3 1.293l.646-.647a.5.5 0 0 1 .708 0L5 1.293l.646-.647a.5.5 0 0 1 .708 0L7 1.293l.646-.647a.5.5 0 0 1 .708 0L9 1.293l.646-.647a.5.5 0 0 1 .708 0l.646.647.646-.647a.5.5 0 0 1 .708 0l.646.647.646-.647a.5.5 0 0 1 .801.13l.5 1A.5.5 0 0 1 15 2v12a.5.5 0 0 1-.053.224l-.5 1a.5.5 0 0 1-.8.13L13 14.707l-.646.647a.5.5 0 0 1-.708 0L11 14.707l-.646.647a.5.5 0 0 1-.708 0L9 14.707l-.646.647a.5.5 0 0 1-.708 0L7 14.707l-.646.647a.5.5 0 0 1-.708 0L5 14.707l-.646.647a.5.5 0 0 1-.708 0L3 14.707l-.646.647a.5.5 0 0 1-.801-.13l-.5-1A.5.5 0 0 1 1 14V2a.5.5 0 0 1 .053-.224l.5-1a.5.5 0 0 1 .367-.27m.217 1.338L2 2.118v11.764l.137.274.51-.51a.5.5 0 0 1 .707 0l.646.647.646-.646a.5.5 0 0 1 .708 0l.646.646.646-.646a.5.5 0 0 1 .708 0l.646.646.646-.646a.5.5 0 0 1 .708 0l.646.646.646-.646a.5.5 0 0 1 .708 0l.646.646.646-.646a.5.5 0 0 1 .708 0l.509.509.137-.274V2.118l-.137-.274-.51.51a.5.5 0 0 1-.707 0L12 1.707l-.646.647a.5.5 0 0 1-.708 0L10 1.707l-.646.647a.5.5 0 0 1-.708 0L8 1.707l-.646.647a.5.5 0 0 1-.708 0L6 1.707l-.646.647a.5.5 0 0 1-.708 0L4 1.707l-.646.647a.5.5 0 0 1-.708 0z"
                />
                <path
                  d="M3 4.5a.5.5 0 0 1 .5-.5h6a.5.5 0 1 1 0 1h-6a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h6a.5.5 0 1 1 0 1h-6a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h6a.5.5 0 1 1 0 1h-6a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h6a.5.5 0 0 1 0 1h-6a.5.5 0 0 1-.5-.5m8-6a.5.5 0 0 1 .5-.5h1a.5.5 0 0 1 0 1h-1a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h1a.5.5 0 0 1 0 1h-1a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h1a.5.5 0 0 1 0 1h-1a.5.5 0 0 1-.5-.5m0 2a.5.5 0 0 1 .5-.5h1a.5.5 0 0 1 0 1h-1a.5.5 0 0 1-.5-.5"
                />
              </svg></button
            ></Motion
          >
        </td>
        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-dark"
              use:motion
              on:click={() => {
                modifier(
                  user["id"],
                  user["nom"],
                  user["prenom"],
                  user["adresse"],
                  user["photo"],
                  user["type_document"],
                  user["code"]
                );
              }}
              ><svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-info-circle"
                viewBox="0 0 16 16"
              >
                <path
                  d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14m0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16"
                />
                <path
                  d="m8.93 6.588-2.29.287-.082.38.45.083c.294.07.352.176.288.469l-.738 3.468c-.194.897.105 1.319.808 1.319.545 0 1.178-.252 1.465-.598l.088-.416c-.2.176-.492.246-.686.246-.275 0-.375-.193-.304-.533zM9 4.5a1 1 0 1 1-2 0 1 1 0 0 1 2 0"
                />
              </svg></button
            ></Motion
          >
        </td>
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
  <p>Aucun rendez-vous prévus</p>
{/if}

<br />
<br />

<div id="modif">
  <!-- modification arrondissement -->
  {#if modif}
    <div>
      <form class="form-control" action="" style="padding: 100px;">
        <p class="title">Information sur le CNI a recuperer</p>
        <div>
          <ul>
            <li class="text">code de vérification : {codes}</li>
            <br />
            <li class="text">Nom : {noms}</li>
            <br />
            <li class="text">Prenom : {prenoms}</li>
            <br />
            <li class="text">Adresse : {adresses}</li>
            <br />
            <li class="text">photo :</li>
          </ul>
          <center>
            <img src={photos} alt="" width="300px" />
          </center>
          <br />
          <li class="text">Motif de demande : {motifs}</li>
        </div>

        <br />
        <div style="display: flex; gap: 30px;">
          <button class="btn btn-success" on:click={handleSubmit2}
            >Recuperer</button
          >
          <button
            class="btn btn-dark"
            on:click={() => {
              window.scrollTo({
                top: 0,
                behavior: "smooth", // Pour un défilement en douceur
              });
              modif = false;
            }}>Fermer</button
          >
        </div>
      </form>
    </div>
  {/if}

  <!-- fin -->
</div>

<style>
  /* css arrondissement */
  .form-control {
    margin: 20px;
    background-color: #ffffff;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
    width: 900px;
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

  .text {
    font-size: 1.5em;
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
