<script>
  // @ts-nocheck

  import { onMount } from "svelte";

  import { goto } from "$app/navigation";
  import Erreur from "../../../Components/Erreur.svelte";
  import HeaderAttenteSuperAdmin from "../../../Components/HeaderAttenteSuperAdmin.svelte";

  import StatPieAdmin from "../../../Components/StatPieAdmin.svelte";
  import StatUser from "../../../Components/StatUser.svelte";
  import FooterAttenteSuperAdmin from "../../../Components/FooterAttenteSuperAdmin.svelte";
  import Chargement from "../../../Components/Chargement.svelte";
  import ProgressStat from "../../../Components/ProgressStat.svelte";

  let loads = true;
  setTimeout(() => {
    loads = false;
  }, 1000);
  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  let val = "";
  let post = [];
  let users = "";
  const getPosts = async () => {
    users = sessionStorage.getItem("admin");

    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/afficheUtilisateur/" + users
    );

    const data = await res.json();
    const filter = data;

    return filter;
  };

  onMount(async () => {
    try {
      let id = sessionStorage.getItem("admin");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      }
      post = await getPosts();
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<title>Menu</title>
<div>
  <HeaderAttenteSuperAdmin men="active" />
  <br />
  <br />
  <br />
  {#await getPosts()}
    <center
      style="height: 400px; display: flex; align-items: center; justify-content: center;"
    >
      <Chargement />
    </center>
  {:then data}
    {#if users != undefined}
      <div style="padding: 0px 30px 0px 100px;">
        <h1>Vos informations</h1>
        <hr />
      </div>
      <br />
      <br />
      <div
        style="display: flex; align-items: center; justify-content: center; flex-direction: column;"
      >
        <div class="info">
          <center> <h2>Information Personnel</h2></center>
          <br /><br />
          <div class="prev">
            <div class="photos">
              <img
                src={filter(data["photo"])}
                alt=""
                srcset=""
                class="photo"
                style=" max-width: 100%; /* Empêcher l'image de dépasser la largeur de son conteneur */
            height: auto; "
              />
            </div>
            <div class="prop">
              <p>Identifiant : {data["identifiant"]}</p>
              <p>Email : {data["email"]}</p>
              <p>Tel : {data["tel"]}</p>
              <div style="display: flex;">
                <p style="">Mots de passe :</p>
                <p style="overflow: hidden; width: 300px;">
                  {data["password"]}
                </p>
              </div>
            </div>
          </div>
          <br />
          <div class="bones">
            <a
              href="/SuperAdmin/ModifSuperAdmin"
              class="btn btn-dark"
              style="height:50px; width:200px; margin-right: 20px;">Modifier</a
            >
            <button
              class="btn btn-outline-danger"
              style="height:50px; width:200px; margin-right: 20px;"
              on:click={() => {
                sessionStorage.removeItem("admin");
                goto("/Utilisateur/Connexion");
              }}>Déconnecter</button
            >
          </div>
        </div>
        <br />
        <br />
        <br />
      </div>
      <div
        style="display: flex; justify-content: center; width: 100%; flex-direction: column; gap:20px; align-items: center;"
      >
        <div class="stat1">
          <br />
          <center>
            <h3>Information sur les personnes inscrit</h3>
          </center>
          <br />
          <StatPieAdmin />
        </div>
        <div class="stat1">
          <center><h3>Les personnes inscrit chaque mois</h3></center>
          <StatUser nom="Chart2" />
        </div>
      </div>
      <br /><br />
      <div
        style="display: flex; align-items: center; justify-content: center; flex-direction: column; width: 100%"
      >
        <div class="stat1">
          <br />
          <center>
            <h4>Un Aperçu de la Performance de Mada Identité</h4>
          </center>
          <br />
          <div style="width: 80%;">
            <ProgressStat />
          </div>
          <br />
        </div>
      </div>
    {:else}
      <Erreur />
    {/if}
  {/await}
  <br />
  <br />
  <br />
  <FooterAttenteSuperAdmin />
</div>

<style>
  .prev {
    display: flex;
    justify-content: space-between;
  }
  h1 {
    font-size: 2rem;
    font-weight: bold;
  }
  p {
    font-size: 1.4em;
  }
  h2 {
    font-size: 1.7rem;
    font-weight: bold;
    text-decoration: underline;
  }
  .info {
    width: 90%;
    border: 3px solid black;
    border-radius: 3px 3px 20px 20px;
    padding: 30px 10px 40px 30px;
  }
  .photo {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    padding: 20px;
    border-radius: 20px;
  }
  .prop {
    width: 70%;
  }
  .bones {
    display: flex;
    justify-content: right;
    padding: 20px;
  }
  .stat1 {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 20px 10px 10px 10px;
    width: 80%;

    border-radius: 3px 3px 20px 20px;
    border: 3px solid black;
  }

  @media only screen and (max-width: 768px) {
    p {
      font-size: 1em;
    }
    h1 {
      font-size: 1.7rem;
    }
    .prev {
      flex-direction: column;
      align-items: center;
    }
    .photo {
      margin-bottom: 30px;
    }
    .bones {
      justify-content: center;
    }
  }
</style>
