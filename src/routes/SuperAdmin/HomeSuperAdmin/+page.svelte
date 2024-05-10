<script>
  import StatUser from "../../../Components/StatUser.svelte";
  import ListeDemande from "../../../Components/ListeDemande.svelte";

  import FooterAttenteAdmin from "../../../Components/FooterAttenteAdmin.svelte";
  import HeaderAttenteSuperAdmin from "../../../Components/HeaderAttenteSuperAdmin.svelte";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import StatPieAdmin from "../../../Components/StatPieAdmin.svelte";
  import Chargement from "../../../Components/Chargement.svelte";
  import Rate from "../../../Components/Rate.svelte";
  import ProgressStat from "../../../Components/ProgressStat.svelte";

  let loads = true;
  setTimeout(() => {
    loads = false;
  }, 1000);

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
  let stat = true;
</script>

<div>
  <HeaderAttenteSuperAdmin acc="active" />
  <br /><br />
  {#if !loads}
    <div
      style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="stat">
        {#if stat}
          <div class="titre">
            <div>
              <h1>Statistique Principal</h1>
            </div>
            <button
              class="bg-dark"
              style="border: none; width: 100%;"
              on:click={() => {
                stat = false;
              }}
            >
              <h4 class="text-white">Avis des utilisateurs</h4>
            </button>
          </div>
        {/if}
        {#if !stat}
          <div class="titre">
            <button
              class="bg-dark"
              style="border: none; width: 100%;"
              on:click={() => {
                stat = true;
              }}
            >
              <h4 class="text-white">Statistique Principal</h4>
            </button>
            <div>
              <h1>Avis des utilisateurs</h1>
            </div>
          </div>
        {/if}
        <br />
        <br />
        {#if stat}
          <div
            style="height: 370px; display: flex; align-items: center; justify-content: center; flex-direction: column;"
          >
            <StatPieAdmin />
          </div>
        {/if}
        {#if !stat}
          <div style="width: 70%; height: 370px;">
            <ProgressStat />
          </div>
        {/if}
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
  {/if}
  <div
    style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
  >
    {#if loads}
      <Chargement />
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
  .stat {
    border: 3px solid #343a40;
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
    display: flex;
    justify-content: space-between;
  }

  .titre div h1 {
    padding: 10px;
    width: 100%;
    height: 100%;
    font-size: 1.4rem;
    font-weight: bold;
    color: #343a40;
    text-align: center;
  }
  .titre div {
    width: 100%;
  }
</style>
