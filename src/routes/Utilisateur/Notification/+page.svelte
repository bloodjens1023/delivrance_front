<script>
  // @ts-nocheck

  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";

  // @ts-nocheck

  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import Notif from "../../../Components/Notif.svelte";
  import { onDestroy, onMount } from "svelte";

  let contenues = [];

  async function fetchNotif() {
    try {
      let id = sessionStorage.getItem("identifiant");
      if (id == undefined || id == "") {
        goto("/Error");
      } else {
        let co = [];
        try {
          const response = await fetch(
            "https://bloodjens.pythonanywhere.com/api_notification/" + id
          );
          const data = await response.json();
          co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
          contenues = co;
        } catch (error) {
          console.error(
            "Erreur lors de la récupération des Utilisateur:",
            error
          );
        }
      }
    } catch (error) {
      goto("/Error");
    }
  }

  onMount(() => {
    fetchNotif();
  });
  const u = setInterval(() => {
    fetchNotif();
  }, 1000);

  onDestroy(() => {
    clearInterval(u);
  });
</script>

<div>
  <HeaderAttente notif="active" />
  <br /><br /><br />
  <div
    style="width: 100%; display: flex; align-items: center; justify-content: center; flex-direction: column;"
  >
    <div
      style="width: 90%; height: 100%; border: 4px solid black; padding: 30px; border-radius: 0px 0px 30px 30px;"
    >
      <h1 style="font-weight: bold;">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="32"
          height="32"
          fill="currentColor"
          class="bi bi-bell-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="M8 16a2 2 0 0 0 2-2H6a2 2 0 0 0 2 2m.995-14.901a1 1 0 1 0-1.99 0A5 5 0 0 0 3 6c0 1.098-.5 6-2 7h14c-1.5-1-2-5.902-2-7 0-2.42-1.72-4.44-4.005-4.901"
          />
        </svg><span style="margin-left: 10px;">Notification</span>
      </h1>
      <br /><br /><br />
      {#await contenues}
        <Chargement />
      {:then conts}
        {#each conts as cont}
          <Notif contenu={cont} />
          <br />
        {/each}
      {/await}
      <br />
      <section class="dots-container">
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
      </section>
    </div>
  </div>
  <br /><br /><br /><br /><br />
  <FooterAttenteUtilisateur />
</div>

<style>
  .dots-container {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    width: 100%;
  }

  .dot {
    height: 15px;
    width: 15px;
    margin-right: 10px;
    border-radius: 10px;
    background-color: #000000;
    animation: pulse 1.5s infinite ease-in-out;
  }

  .dot:last-child {
    margin-right: 0;
  }

  .dot:nth-child(1) {
    animation-delay: -0.3s;
  }

  .dot:nth-child(2) {
    animation-delay: -0.1s;
  }

  .dot:nth-child(3) {
    animation-delay: 0.1s;
  }

  @keyframes pulse {
    0% {
      transform: scale(0.8);
      background-color: #000000;
      box-shadow: 0 0 0 0 rgba(115, 115, 115, 0.7);
    }

    50% {
      transform: scale(1.2);
      background-color: #000000;
      box-shadow: 0 0 0 10px rgba(178, 212, 252, 0);
    }

    100% {
      transform: scale(0.8);
      background-color: #3c3c3c;
      box-shadow: 0 0 0 0 rgba(139, 139, 140, 0.7);
    }
  }
</style>
