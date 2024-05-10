<script>
  import { onMount } from "svelte";
  import ArrondListe from "../../../Components/ArrondListe.svelte";
  import HeaderAttenteSuperAdmin from "../../../Components/HeaderAttenteSuperAdmin.svelte";
  import { goto } from "$app/navigation";
  import FooterAttenteSuperAdmin from "../../../Components/FooterAttenteSuperAdmin.svelte";
  import Chargement from "../../../Components/Chargement.svelte";
  import DistrictListe from "../../../Components/DistrictListe.svelte";
  import RegiontListe from "../../../Components/RegiontListe.svelte";

  let loading = true;
  let region = true;
  let district = false;
  let arrond = false;
  setTimeout(() => {
    loading = false;
  }, 1500);
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
  <HeaderAttenteSuperAdmin arr="active" />
  <br /><br />
  <div
    style="width: 100%; display:flex;align-items: center; justify-content: center;flex-direction: column;"
  >
    {#if loading}
      <Chargement />
    {/if}
    {#if !loading}
      <center>
        <div class="radio-inputs">
          <label class="radio">
            <input
              type="radio"
              name="radio"
              checked
              on:click={() => {
                region = true;
                district = false;
                arrond = false;
              }}
            />
            <span class="name">Région</span>
          </label>
          <label class="radio">
            <input
              type="radio"
              name="radio"
              on:click={() => {
                region = false;
                district = true;
                arrond = false;
              }}
            />
            <span class="name">District</span>
          </label>
          <label class="radio">
            <input
              type="radio"
              name="radio"
              on:click={() => {
                region = false;
                district = false;
                arrond = true;
              }}
            />
            <span class="name">Arrondissement</span>
          </label>
        </div>
      </center>
      <br /><br /><br />
      {#if arrond}
        <ArrondListe />
      {/if}
      {#if region}
        <RegiontListe />
      {/if}
      {#if district}
        <DistrictListe />
      {/if}
    {/if}
  </div>

  <br /><br /><br /><br /><br />
  <FooterAttenteSuperAdmin />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins", "Helvetica";
  }
  .radio-inputs {
    position: relative;
    display: flex;
    flex-wrap: wrap;
    border-radius: 0.5rem;
    background-color: #eee;
    box-sizing: border-box;
    box-shadow: 0 0 0px 1px rgba(0, 0, 0, 0.06);
    padding: 0.25rem;
    width: 600px;
    font-size: 14px;
  }

  .radio-inputs .radio {
    flex: 1 1 auto;
    text-align: center;
  }

  .radio-inputs .radio input {
    display: none;
  }

  .radio-inputs .radio .name {
    display: flex;
    cursor: pointer;
    align-items: center;
    justify-content: center;
    border-radius: 0.5rem;
    border: none;
    padding: 0.5rem 0;
    color: rgba(51, 65, 85, 1);
    transition: all 0.15s ease-in-out;
  }

  .radio-inputs .radio input:checked + .name {
    background-color: #fff;
    font-weight: 600;
  }
</style>
