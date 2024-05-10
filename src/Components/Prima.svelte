<script>
  // @ts-nocheck
  import { goto } from "$app/navigation";

  import { Motion } from "svelte-motion";

  export let donne = [];
  export let user = "";

  function filter(a) {
    return (
      "https://www.pythonanywhere.com/user/bloodjens/files/home/bloodjens/" + a
    );
  }
  const getPosts = async () => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/supprimerDocument/" + user
    );

    const data = await res.json();
    const filter = data;
    if (filter) {
      goto("/Utilisateur/Attente");
      return true;
    } else {
      return false;
    }
  };
</script>

<div>
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
          class="bi bi-clipboard"
          viewBox="0 0 16 16"
        >
          <path
            d="M4 1.5H3a2 2 0 0 0-2 2V14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V3.5a2 2 0 0 0-2-2h-1v1h1a1 1 0 0 1 1 1V14a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V3.5a1 1 0 0 1 1-1h1z"
          />
          <path
            d="M9.5 1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-3a.5.5 0 0 1-.5-.5v-1a.5.5 0 0 1 .5-.5zm-3-1A1.5 1.5 0 0 0 5 1.5v1A1.5 1.5 0 0 0 6.5 4h3A1.5 1.5 0 0 0 11 2.5v-1A1.5 1.5 0 0 0 9.5 0z"
          />
        </svg><span style="margin-left: 10px;">Votre demande</span>
      </h1>
      <br /><br /><br />
      <div class="left">
        <h4>Nom: {donne["nom"]}</h4>
        <h4>Prenom: {donne["prenom"]}</h4>
        <h4>Adresse: {donne["adresse"]}</h4>
        <h4>Photo d'identité:</h4>
        <div
          style="display: flex; align-items: center; justify-content: center; margin-bottom: 100px;"
        >
          <img src={filter(donne["photo"])} alt="" srcset="" height="400px" />
        </div>
        <h4>Copie d'acte de naissance:</h4>
        <div
          style="display: flex; align-items: center; justify-content: center; margin-bottom: 100px;"
        >
          <img
            src={filter(donne["acteNaissance"])}
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
            src={filter(donne["certificat"])}
            alt=""
            srcset=""
            height="400px"
          />
        </div>
      </div>
      <div
        style="width: 100%; display: flex; gap:30px; align-items: center; justify-content: center;"
      >
        <Motion
          let:motion
          whileHover={{ scale: 1.2 }}
          whileTap={{ scale: 1.1 }}
        >
          <a class="btn btn-dark" use:motion href="/Utilisateur/WeltStep/"
            >Modifier</a
          >
        </Motion>

        <Motion
          let:motion
          whileHover={{ scale: 1.2 }}
          whileTap={{ scale: 1.1 }}
        >
          <button
            class="btn btn-outline-danger"
            use:motion
            on:click={() => {
              console.log(getPosts());
            }}>Supprimer</button
          >
        </Motion>
      </div>
      <br /><br /><br />
    </div>
  </div>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  h4 {
    margin-bottom: 20px;
    font-size: 1.5em;
    font-family: "Poppins", "Helvetica";
  }
  .left {
    padding-left: 100px;
  }
  @media only screen and (max-width: 768px) {
    h4 {
      font-size: 1em;
    }
    .left {
      padding-left: 10px;
    }
  }
</style>
