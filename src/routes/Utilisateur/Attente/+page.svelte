<script>
  // @ts-nocheck

  import { onDestroy, onMount } from "svelte";
  import Card from "../../../Components/Card.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import { goto } from "$app/navigation";

  let users = [];
  let id = "";
  let filtrer = users;
  const interval = setInterval(fetchUtilisateur, 500);
  async function fetchUtilisateur() {
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_affiche_pub/"
      );
      const data = await response.json();
      users = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      filtrer = users;
      console.log(filtrer);
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateurs:", error);
    }
  }

  onDestroy(() => {
    clearInterval(interval);
  });

  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  onMount(() => {
    try {
      id = sessionStorage.getItem("identifiant");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        window.scrollTo({
          top: 0,
          behavior: "smooth", // Pour un défilement en douceur
        });
        fetchUtilisateur();
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  <HeaderAttente acc="active" />
  <br /><br /><br />
  <div
    style="width: 100%; display: flex; align-items: center; justify-content: center; flex-direction: column;"
  >
    {#each filtrer as user}
      <Card
        description={user["description"]}
        aimer={user["aimer"]}
        image={filter(user["photo"])}
        ids={user["id"]}
        detail={false}
      />
      <br /><br /><br />
    {/each}
    {#if filtrer.length == 0}
      <div
        style="height: 50vh; display: flex; align-items: center; justify-content: center;"
      >
        <h1>Aucun nouveau disponible</h1>
      </div>
    {/if}
  </div>
  <br /><br /><br /><br /><br />
  <FooterAttenteUtilisateur />
</div>
