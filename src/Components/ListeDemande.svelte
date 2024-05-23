<script>
  // @ts-nocheck
  import { onDestroy, onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import Modal from "./Modal.svelte";
  import toast, { Toaster } from "svelte-french-toast";

  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  let donne = {
    un: "",
    deux: "",
    trois: "",
    quatre: "",
    cinq: "",
    six: "",
  };
  export let contenu = { nom: "", prenom: "", cni: "", date: "", type: "" };
  let detail = false;
  let co = [];
  let showModal = false;
  let nuller = false;
  let accepter = false;
  let motif = "";
  let err = false;
  var dateActuelle = new Date();
  let date = dateActuelle.getDate();
  let temp = undefined;

  async function fetchDemande() {
    try {
      let formdata = new FormData();
      formdata.append("motif", motif);
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_refuser_demande/" +
          contenu.id,
        {
          method: "POST",
          body: formdata,
        }
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON

      if (contenu.length == 0) {
        nuller = true;
      }
      toast.success("La demande a été refuser", {
        style: "font-size:15px; padding:10px",
        duration: 2000,
      });
    } catch (error) {
      toast.error("Erreur de serveur", {
        style: "font-size:15px; padding:10px",
        duration: 2000,
      });
    }
  }

  async function accepterDemande() {
    genererCarte();
    try {
      donne.un = Math.floor(Math.random() * 9).toString();
      donne.deux = Math.floor(Math.random() * 9).toString();
      donne.trois = Math.floor(Math.random() * 9).toString();
      donne.quatre = Math.floor(Math.random() * 9).toString();
      donne.cinq = Math.floor(Math.random() * 9).toString();
      donne.six = Math.floor(Math.random() * 9).toString();
      let val =
        donne.un +
        "" +
        donne.deux +
        "" +
        donne.trois +
        "" +
        donne.quatre +
        "" +
        "" +
        donne.cinq +
        "" +
        "" +
        donne.six +
        "";

      let formdata = new FormData();
      formdata.append("date", date);
      formdata.append("temp", temp);
      formdata.append("code", val);
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_accepter_demande/" +
          contenu.id,
        {
          method: "POST",
          body: formdata,
        }
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      if (contenu.length == 0) {
        nuller = true;
      }
      toast.success("Demande accepté", {
        style: "font-size:15px; padding:10px",
        duration: 2000,
      });
    } catch (error) {
      toast.error("Erreur de serveur", {
        style: "font-size:15px; padding:10px",
        duration: 2000,
      });
    }
  }

  async function genererCarte() {
    try {
      let formdata = new FormData();
      formdata.append("id_document", contenu.id);
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/gerer_cni/" + contenu.id,
        {
          method: "POST",
          body: formdata,
        }
      );
    } catch (error) {
      toast.error("Erreur de serveur", {
        style: "font-size:15px; padding:10px",
        duration: 2000,
      });
    }
  }

  onMount(() => {
    if (contenu.length == 0) {
      nuller = true;
    } else {
      nuller = false;
    }
  });
</script>

<Toaster />
{#if showModal}
  <Modal bind:showModal>
    <h2 slot="header">⚠ Attention !</h2>
    <hr />
    <br />
    <ol class="definition-list">voulez-vous vraiment refuser cet demande ?</ol>
    <br />
    <center>
      <p>motif de refus :</p>

      <textarea
        name=""
        id=""
        bind:value={motif}
        style="width: 90%; min-height: 150px;"
      ></textarea>
      <br />
      <br />
    </center>
    <hr />
    <div style="display: flex;">
      <button
        class="btn btn-dark"
        on:click={() => {
          showModal = false;

          fetchDemande();
        }}>oui</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = false;
        }}>non</button
      >
    </div>
  </Modal>
{/if}

{#if accepter}
  <Modal showModal={accepter}>
    <h2 slot="header">✅ Attention !</h2>
    <hr />
    <br />
    <ol class="definition-list">voulez-vous vraiment accepter cet demande ?</ol>
    <br />
    <center>
      <p>date et heure de la récupération :</p>
      {#if err}
        <p style="color: red;">Formulaire invalide !!</p>
      {/if}
      <input type="date" name="" id="" bind:value={date} />
      <input type="time" name="" id="" bind:value={temp} />
      <br />
      <br />
    </center>
    <hr />
    <div style="display: flex;">
      <button
        class="btn btn-success"
        on:click={() => {
          const r = new Date();

          const z = new Date(date);

          if (z > r && temp != undefined) {
            accepter = false;
            accepterDemande();

            err = false;
          } else {
            err = true;
          }
        }}>oui</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = false;
          accepter = false;
        }}>non</button
      >
    </div>
  </Modal>
{/if}

{#if nuller}
  <center>
    <p>aucun nouveau demande détécter</p>
  </center>
{/if}

{#if contenu.typeDocument == "primata"}
  <Motion
    let:motion
    whileHover={{ boxShadow: "1px 1px 1px black", scale: 1.001 }}
    whileTap={{ scale: 1 }}
  >
    <div
      style="width: 100%; height: 100%; padding: 40px 10px 10px 40px; border: 2px solid black; cursor: pointer; border-radius: 20px;"
      use:motion
    >
      <div class="prima">
        {#if !detail}
          <div style="width: 70%;">
            <p>Nom : {contenu.nom}</p>
            <p>Prenom : {contenu.prenom}</p>
            {#if contenu.typeDocument != "primata"}
              <p>cni : {contenu.cni}</p>
            {/if}
            <p>date : {contenu.date}</p>
            <p>type de demande : {contenu.typeDocument}</p>
          </div>
          <div>
            <Motion
              let:motion
              whileHover={{ scale: 1.1 }}
              whileTap={{ scale: 1.0 }}
            >
              <button
                use:motion
                class="btn btn-dark"
                on:click={() => {
                  setTimeout(() => {
                    detail = true;
                  }, 100);
                }}>plus de détail</button
              >
            </Motion>
          </div>
        {/if}
        {#if detail}
          <div style="display: block; width: 100%;">
            <div>
              <div
                style="width: 95%; display: flex; align-items: center; justify-content: right; "
              >
                <Motion
                  let:motion
                  whileHover={{ scale: 1.1 }}
                  whileTap={{ scale: 1.0 }}
                >
                  <button
                    use:motion
                    style="background-color: transparent; border: none;"
                    on:click={() => {
                      detail = false;
                    }}
                    ><svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="32"
                      height="32"
                      fill="currentColor"
                      class="bi bi-x-circle-fill"
                      viewBox="0 0 16 16"
                    >
                      <path
                        d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293z"
                      />
                    </svg></button
                  >
                </Motion>
              </div>
            </div>
            <h2 style="font-weight: bold;">Détail de la demande</h2>
            <hr />
            <div class="left" style="margin-left: 30px;">
              <h4>Nom: {contenu.nom}</h4>
              <h4>Prenom: {contenu.prenom}</h4>
              <h4>Adresse: {contenu.adresse}</h4>
              <h4>Photo d'identité:</h4>
              <div
                style="display: flex; align-items: center; justify-content: center; margin-bottom: 100px;"
              >
                <img
                  src={filter(contenu.photo)}
                  alt=""
                  srcset=""
                  height="400px"
                />
              </div>
              <h4>Copie d'acte de naissance:</h4>
              <div
                style="display: flex; align-items: center; justify-content: center; margin-bottom: 100px;"
              >
                <img
                  src={filter(contenu.acteNaissance)}
                  alt=""
                  srcset=""
                  height="400px"
                />
              </div>
              <h4>Cértificat de résidence:</h4>
              <div
                style="display: flex; align-items: center; justify-content: center; margin-bottom: 100px;"
              >
                <img
                  src={filter(contenu.certificat)}
                  alt=""
                  srcset=""
                  height="400px"
                />
              </div>
            </div>
          </div>
        {/if}

        <br />
      </div>
      <button
        class="btn btn-outline-success"
        on:click={() => {
          accepter = true;
        }}>Accepter</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = true;
        }}>Refuser</button
      >
      <br />
      <br />
    </div>
  </Motion>
{/if}
{#if contenu.typeDocument == "duplicatatUse"}
  <Motion
    let:motion
    whileHover={{ boxShadow: "1px 1px 1px black", scale: 1.001 }}
    whileTap={{ scale: 1 }}
  >
    <div
      style="width: 100%; height: 100%; padding: 40px 10px 10px 40px; border: 2px solid black; cursor: pointer; border-radius: 20px;"
      use:motion
    >
      <div class="prima">
        <div style="width: 70%;">
          <p>Nom : {contenu.nom}</p>
          <p>Prenom : {contenu.prenom}</p>
          {#if contenu.typeDocument != "primata"}
            <p>cni : {contenu.numCni}</p>
          {/if}
          <p>date : {contenu.date}</p>
          <p>type de demande : duplicat usée</p>
        </div>
        <div>
          <button class="btn btn-dark">plus de détail</button>
        </div>
        <br />
      </div>
      <button
        class="btn btn-outline-success"
        on:click={() => {
          accepter = true;
        }}>Accepter</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = true;
        }}>Refuser</button
      >
      <br />
      <br />
    </div>
  </Motion>
{/if}

{#if contenu.typeDocument == "duplicatatPerte"}
  <Motion
    let:motion
    whileHover={{ boxShadow: "1px 1px 1px black", scale: 1.001 }}
    whileTap={{ scale: 1 }}
  >
    <div
      style="width: 100%; height: 100%; padding: 40px 10px 10px 40px; border: 2px solid black; cursor: pointer; border-radius: 20px;"
      use:motion
    >
      <div class="prima">
        <div style="width: 70%;">
          <p>Nom : {contenu.nom}</p>
          <p>Prenom : {contenu.prenom}</p>
          {#if contenu.typeDocument != "primata"}
            <p>cni : {contenu.numCni}</p>
          {/if}
          <p>date : {contenu.date}</p>
          <p>type de demande : duplicat perte</p>
        </div>
        <div>
          <button class="btn btn-dark">plus de détail</button>
        </div>
        <br />
      </div>
      <button
        class="btn btn-outline-success"
        on:click={() => {
          accepter = true;
        }}>Accepter</button
      >
      <button
        class="btn btn-danger"
        on:click={() => {
          showModal = true;
        }}>Refuser</button
      >
      <br />
      <br />
    </div>
  </Motion>
{/if}
<br />

<style>
  p {
    font-size: 1.2rem;
    font-weight: bold;
  }
  button {
    margin: 10px;
  }
  .prima {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h4 {
    font-size: 1.2rem;
    font-weight: bold;
    margin-bottom: 30px;
  }
  @media only screen and (max-width: 768px) {
    .prima {
      flex-direction: column;
    }
    p {
      font-size: 1rem;
    }
  }
</style>
