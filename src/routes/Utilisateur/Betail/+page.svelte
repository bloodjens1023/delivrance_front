<script>
  // @ts-nocheck

  import { onMount } from "svelte";
  import Card from "../../../Components/Card.svelte";
  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import { goto } from "$app/navigation";
  let co = [];
  async function fetchComm(id) {
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_affiche_pub_simple/" + id
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      console.log(co);
    } catch (error) {
      console.error("Erreur lors de la récupération des actualité:", error);
    }
  }

  function filter(a) {
    return (
      "https://www.pythonanywhere.com/user/bloodjens/files/home/bloodjens/" + a
    );
  }
  onMount(() => {
    try {
      let id = sessionStorage.getItem("identifiant");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      }
    } catch (error) {
      goto("/Error");
    }
  });
  onMount(() => {
    let u = sessionStorage.getItem("pub_id");
    fetchComm(u);
    console.log(co);
  });
</script>

<div>
  <HeaderAttente acc="active" />
  <br /><br /><br />
  <div
    style="width: 100%; display: flex; align-items: center; justify-content: center; flex-direction: column;"
  >
    {#each co as user}
      <Card
        description={user["description"]}
        aimer={user["aimer"]}
        image={filter(user["photo"])}
        ids={user["id"]}
        detail={true}
      />
      <br /><br /><br />
    {/each}
  </div>
  <br /><br /><br /><br /><br />
  <FooterAttenteUtilisateur />
</div>
