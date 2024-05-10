<script>
  // @ts-nocheck
  import { Motion } from "svelte-motion";
  import ChargementConnect from "./ChargementConnect.svelte";
  import { onMount, onDestroy } from "svelte";
  import Rate from "./Rate.svelte";
  import Chargement from "./Chargement.svelte";
  import HeaderAttente from "./HeaderAttente.svelte";

  export let donne = {};
  export let resultat = "accepter";
  $: cliquer = false;
  let un = "";
  let deux = "";
  let trois = "";
  let quatre = "";
  let cinq = "";
  let six = "";
  let date = "";
  let heure = "";
  let recuperer = "";
  export let user = sessionStorage.getItem("identifiant");
  let filtre;
  const getPosts1 = async () => {
    try {
      const res = await fetch(
        "https://bloodjens.pythonanywhere.com/afficheRdv/" + user
      );

      const data = await res.json();
      filtre = data.data[0];
      un = spliter(filtre["code"])[0];
      deux = spliter(filtre["code"])[1];
      trois = spliter(filtre["code"])[2];
      quatre = spliter(filtre["code"])[3];
      cinq = spliter(filtre["code"])[4];
      six = spliter(filtre["code"])[5];
      date = filtre["date_fin"];
      heure = filtre["heure"];
      recuperer = filtre["recuperer"];
      if (filtre) {
        return true;
      } else {
        return false;
      }
    } catch (e) {}
  };
  onMount(() => {
    getPosts1();
  });

  function spliter(u) {
    u.split();

    return u;
  }
</script>

<div style="display: flex; align-items: center; justify-content: center;">
  {#if resultat == "accepter" && !recuperer}
    <div class="contenu">
      <center><h1>resultat de votre demande de cni</h1></center>
      <br /><br />
      <p>bonjour {donne.nom} {donne.prenom}</p>
      <p>
        Après l'analyse de votre dossier, votre de demande de carte national
        d'identité du {donne.dateDebut} à été approuvé par l'administrateur. afin
        de pouvoir recuperer votre CNI recuperer le code de récuperation ci-dessus
        et rendez-vous sur place le {date} à {heure}.
      </p>
      <p>Merci de vorte attention.</p>
      <p>cordiallement</p>
      <center><p>Ministère de l'intérieur</p></center>
      <br /><br />
      {#if cliquer}
        {#await getPosts1}
          <Chargement />
        {:then donn}
          <center>
            <form class="form">
              <div class="content">
                <div class="inp">
                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={un}
                  />
                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={deux}
                  />

                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={trois}
                  />
                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={quatre}
                  />
                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={cinq}
                  />
                  <input
                    maxlength="1"
                    class="input"
                    type="text"
                    placeholder=""
                    disabled
                    bind:value={six}
                  />
                </div>
              </div>
            </form>
          </center>
          <br /><br />
        {/await}
      {/if}
      <center>
        <Motion let:motion whileTap={{ scale: 1 }} whileHover={{ scale: 1.2 }}>
          <button
            class="btn btn-dark"
            use:motion
            on:click={() => {
              cliquer = !cliquer;
            }}>code de recupération</button
          >
        </Motion>
      </center>
    </div>
  {/if}
  {#if recuperer}
    <center>
      <Rate />
    </center>
  {/if}
  {#if resultat == "refuser"}
    <div class="contenu">
      <center><h1>resultat de votre demande de cni</h1></center>
      <br /><br />
      <p>bonjour {donne.nom} {donne.prenom}</p>
      <p>
        Après l'analyse de votre dossier, votre de demande de carte national
        d'identité du 10/10/24 à été refuser par l'administrateur. Plusieur
        raison nous ont pousser a le decliner qui sont:
      </p>
      <ul>
        <li>un</li>
        <li>deux</li>
        <li>trois</li>
      </ul>
      <p>Merci de vorte attention.</p>
      <p>cordiallement</p>
      <center><p>Ministère de l'intérieur</p></center>
      <br /><br />
      <center>
        <Motion let:motion whileTap={{ scale: 1 }} whileHover={{ scale: 1.2 }}>
          <button class="btn btn-dark" use:motion
            >envoyer un nouveau demande</button
          >
        </Motion>
      </center>
    </div>
  {/if}
</div>

<br /><br /><br />

<style>
  @import url("$lib/bootstrap.min.css");
  .contenu {
    border: 3px solid black;
    width: 90%;
    padding: 40px;
  }
  h1 {
    font-size: 2em;
    font-variant: small-caps;
    font-weight: bolder;
    text-decoration: underline;
  }
  p {
    font-size: 1.2em;
    font-variant: small-caps;
    font-weight: bolder;
  }

  .form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    background: white;
    border-radius: 30px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(8.2px);
    -webkit-backdrop-filter: blur(8.2px);
    border: 1px solid #fff;
    width: auto;
    max-width: 400px;
    height: 12em;
  }

  .content {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-top: auto;
    margin-bottom: auto;
  }

  .inp {
    margin-left: auto;
    margin-right: auto;
    white-space: 4px;
  }

  .input + .input {
    margin-left: 0.5em;
  }

  .input {
    color: #000000;
    height: 2em;
    width: 2em;
    float: left;
    text-align: center;

    outline: none;
    border: 3px #000000 solid;
    border-radius: 5px;
    transition: all 0.6s ease;
    font-size: 1.5em;
  }

  .input:focus {
    outline: none;
    border: 1px #fff solid;
  }
</style>
