<script>
  // @ts-nocheck

  import { onDestroy, onMount } from "svelte";
  import FooterAttenteAdmin from "../../../Components/FooterAttenteAdmin.svelte";
  import HeaderAttenteAdmin from "../../../Components/HeaderAttenteAdmin.svelte";
  import ListeDemande from "../../../Components/ListeDemande.svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";

  let co = [];
  let filtrer = co;
  $: actuel = [];
  let loading = true;
  setTimeout(() => {
    loading = false;
  }, 1500);
  async function fetchDemande() {
    let id;
    id = sessionStorage.getItem("chef");

    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_liste_demande_arrond/" + id
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      filtrer = co;
      totalPages = Math.ceil(filtrer.length / itemsPerPage);
      actuel = getCurrentPageData();
    } catch (error) {
      console.error("Erreur lors de la récupération des demandes:", error);
    }
  }

  try {
    let u = setInterval(() => {
      fetchDemande();
    }, 2000);

    onDestroy(() => {
      clearInterval(u);
    });
  } catch (error) {}

  let itemsPerPage = 5; // Nombre d'éléments par page
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
  onMount(() => {
    try {
      let id = sessionStorage.getItem("chef");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        fetchDemande();
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  <HeaderAttenteAdmin dem="active" />
  <br /><br />

  {#if loading}
    <Chargement />
  {/if}
  {#if !loading}
    <div
      style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="stat" style="padding: 50px; width: 80%;">
        <div class="titre">
          <div>
            <h2>Liste des demandes</h2>
          </div>
          <hr />
        </div>
        <br />
        <br />
        {#each actuel as conts}
          <ListeDemande contenu={conts} />
        {/each}
        {#if co.length == 0}
          <ListeDemande contenu={co} />
        {/if}

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
      </div>
    </div>
  {/if}
  <br /><br /><br /><br /><br />
  <FooterAttenteAdmin />
</div>

<style>
  h2 {
    font-weight: bold;
  }
</style>
