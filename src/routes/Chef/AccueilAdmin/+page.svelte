<script>
  // @ts-nocheck

  import HeaderAttenteAdmin from "../../../Components/HeaderAttenteAdmin.svelte";
  import StatUser from "../../../Components/StatUser.svelte";
  import ListeDemande from "../../../Components/ListeDemande.svelte";
  import StatPie from "../../../Components/StatPie.svelte";
  import FooterAttenteAdmin from "../../../Components/FooterAttenteAdmin.svelte";
  import { onDestroy, onMount } from "svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";

  let id = "";
  let co = [];
  async function fetchDemande() {
    try {
      let id = sessionStorage.getItem("chef");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        try {
          const response = await fetch(
            "https://bloodjens.pythonanywhere.com/api_liste_demande_arrond/" +
              id
          );
          const data = await response.json();
          co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
        } catch (error) {
          console.error("Erreur lors de la récupération des demandes:", error);
        }
      }
    } catch (error) {
      goto("/Error");
    }
  }

  try {
    let u = setInterval(() => {
      fetchDemande();
    }, 500);

    onDestroy(() => {
      clearInterval(u);
    });
  } catch (error) {}

  onMount(() => {
    try {
      id = sessionStorage.getItem("chef");

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
  <HeaderAttenteAdmin acc="active" />
  <br /><br />
  {#await fetchDemande()}
    <Chargement />
  {:then dats}
    <div
      style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="stat">
        <div class="titre">
          <div>
            <h1>Statistique Principal</h1>
          </div>
          <div></div>
        </div>
        <br />
        <br />
        <center>
          <StatPie />
        </center>
        <br />
      </div>
    </div>
    <br /><br />
    <div
      style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="stat">
        <div class="titre">
          <div>
            <h1>Statistique</h1>
          </div>
        </div>
        <br />
        <br />
        <div style="width: 90%; padding: 10px;">
          <StatUser />
        </div>
      </div>
    </div>

    <br /><br />
    <div
      style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="stat" style="padding: 50px; width: 80%;">
        <div class="titre">
          <div>
            <h2>Liste des demandes</h2>
          </div>
        </div>
        <br />
        <br />
        {#each co as conts}
          <ListeDemande contenu={conts} />
        {/each}

        <br />
        <a href="/DemandeAdmin" class="btn btn-dark">Voir plus</a>
      </div>
    </div>
  {/await}
  <br /><br /><br /><br /><br />
  <FooterAttenteAdmin />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins", "Helvetica";
  }
  .stat {
    border: 3px solid black;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    width: 60%;
    border-radius: 0px 0px 30px 30px;
  }
  .titre {
    width: 100%;

    font-size: 1.4rem;
    font-weight: bold;
    background-color: black;
    display: flex;
    justify-content: space-between;
  }

  .titre div h1 {
    padding: 10px;
    width: 100%;
    height: 100%;
    font-size: 1.4rem;
    font-weight: bold;
    color: black;
    background-color: white;
    text-align: center;
  }
  .titre div h2 {
    padding: 10px;
    width: 100%;
    height: 100%;
    font-size: 1.9rem;
    font-weight: bold;
    color: black;
    text-align: center;
    background-color: white;
  }
  .titre div {
    width: 100%;
  }
</style>
