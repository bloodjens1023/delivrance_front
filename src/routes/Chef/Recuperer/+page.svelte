<script>
  import { onMount } from "svelte";
  import ArrondListe from "../../../Components/ArrondListe.svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";
  import FooterAttenteAdmin from "../../../Components/FooterAttenteAdmin.svelte";
  import HeaderAttenteAdmin from "../../../Components/HeaderAttenteAdmin.svelte";
  import RdvListe from "../../../Components/RdvListe.svelte";

  let loading = true;
  setTimeout(() => {
    loading = false;
  }, 1500);
  onMount(() => {
    try {
      let id = sessionStorage.getItem("chef");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  <HeaderAttenteAdmin notif="active" />
  <br /><br />
  <div
    style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
  >
    {#if loading}
      <Chargement />
    {/if}
    {#if !loading}
      <RdvListe />
    {/if}
  </div>

  <br /><br /><br /><br /><br />
  <FooterAttenteAdmin />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins", "Helvetica";
  }
</style>
