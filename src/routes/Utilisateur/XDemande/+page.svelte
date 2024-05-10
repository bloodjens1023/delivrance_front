<script>
  // @ts-nocheck

  import Footer from "../../../Components/Footer.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import DupliUse from "../../../Components/DupliUse.svelte";
  import DupliPerte from "../../../Components/DupliPerte.svelte";
  import Prima from "../../../Components/Prima.svelte";
  import Erreur from "../../../Components/Erreur.svelte";

  import { onDestroy, onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";
  import ResultatDemande from "../../../Components/ResultatDemande.svelte";

  $: post = {
    nom: "",
    prenom: "",
    adresse: "",
    numCni: "",
    photo: "",
    certificate: "",
    declarationPerte: "",
    acteNaissance: "",
    etatDocument: "",
    typeDocument: "",
  };

  let val = "";

  let load = true;

  let users = "";
  const getPosts = async () => {
    try {
      users = sessionStorage.getItem("identifiant");

      if (users == null || users == undefined || users == "") {
        goto("/Error");
      }
      const res = await fetch(
        "https://bloodjens.pythonanywhere.com/afficheDocument/" + users
      );

      const data = await res.json();

      post = data;
      console.log(post);
      // let r = filter.split("/");
      // r.shift();
      // let z = r.join("/");
    } catch (error) {
      goto("/Error");
    }
  };

  onMount(async () => {
    // @ts-ignore

    post = getPosts();
  });
</script>

<div>
  <HeaderAttente dem="active" />

  {#if users == undefined}
    <Erreur />
  {:else}
    <br /><br /><br />
    {#if post["etatDocument"] == "encours"}
      {#if post["typeDocument"] == "duplicatatUse"}
        <DupliUse donne={post} user={users} />
        <br /><br /><br /><br /><br />
      {/if}
      {#if post["typeDocument"] == "duplicatatPerte"}
        <DupliPerte donne={post} user={users} />
        <br /><br /><br /><br /><br />
      {/if}

      {#if post["typeDocument"] == "primata"}
        <Prima donne={post} user={users} />
        <br /><br /><br /><br /><br />
      {/if}
    {/if}
    {#if post["etatDocument"] == "accepter"}
      <ResultatDemande donne={post} resultat={"accepter"} user={users} />
    {/if}
    {#if post["etatDocument"] == "refuser"}
      <ResultatDemande donne={post} resultat="refuser" user={users} />
    {/if}
    {#if post["donne"] == "aucun"}
      <center
        style="height: 70vh; display: flex; align-items: center; justify-content: center; flex-direction: column; gap:30px"
      >
        <div class="loader"></div>
        <br />
        <Motion
          let:motion
          whileHover={{ scale: 1.2 }}
          whileTap={{ scale: 1.1 }}
        >
          <a class="btn btn-success" href="/Utilisateur/Step" use:motion
            >envoyer un demande</a
          >
        </Motion>
      </center>
    {/if}
  {/if}

  <FooterAttenteUtilisateur />
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins";
  }
  .loader {
    width: fit-content;
    font-weight: bold;
    font-family: monospace;
    font-size: 30px;
    background: radial-gradient(circle closest-side, #000 94%, #0000)
      right/calc(200% - 1em) 100%;
    animation: l24 3s infinite alternate linear;
  }

  .loader::before {
    content: "Aucun Demande identifier";
    line-height: 1em;
    color: #0000;
    background: inherit;
    background-image: radial-gradient(circle closest-side, #fff 94%, #000);
    -webkit-background-clip: text;
    background-clip: text;
  }

  @keyframes l24 {
    100% {
      background-position: left;
    }
  }

  @media only screen and (max-width: 768px) {
    .loader::before {
      content: "Aucun Demande identifier";
      line-height: 1em;
      color: #0000;
      background: inherit;
      background-image: transparent;
      -webkit-background-clip: text;
      background-clip: text;
    }
  }
</style>
