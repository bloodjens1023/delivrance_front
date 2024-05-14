<script>
  import HeaderAttenteSuperAdmin from "../../../Components/HeaderAttenteSuperAdmin.svelte";
  import UtilisateurListe from "../../../Components/UtilisateurListe.svelte";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import FooterAttenteSuperAdmin from "../../../Components/FooterAttenteSuperAdmin.svelte";
  import Chargement from "../../../Components/Chargement.svelte";

  onMount(() => {
    try {
      let id = sessionStorage.getItem("admin");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  <HeaderAttenteSuperAdmin dem="active" />
  <br /><br />
  <div
    style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
  >
    <UtilisateurListe />
  </div>

  <br /><br /><br /><br /><br />
  <FooterAttenteSuperAdmin />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins", "Helvetica";
  }
</style>
