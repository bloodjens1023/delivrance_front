<script>
  // @ts-nocheck

  import { Motion } from "svelte-motion";
  import { onMount } from "svelte";
  let donne = {};
  let success = false;
  let error = false;
  let loads = false;
  export let onSubmit = () => {};
  export let update = false;
  const handleSubmit = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    formdata.append("type", "duplicatatPerte");
    formdata.append("nom", donne.nom);
    formdata.append("prenom", donne.prenom);
    formdata.append("numCni", donne.num);
    formdata.append("adresse", donne.adresse);
    formdata.append("photo", donne.photo);
    formdata.append("certificat", donne.resid);
    formdata.append("declarationPerte", donne.perte);
    formdata.append("identifiant", sessionStorage.getItem("identifiant"));

    let response;
    if (update) {
      let use = sessionStorage.getItem("identifiant");
      response = await fetch(
        "https://bloodjens.pythonanywhere.comhonanywhere.com/updateDocument/" +
          use,
        {
          method: "POST",
          body: formdata,
        }
      );
    } else {
      response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_demande/",
        {
          method: "POST",
          body: formdata,
        }
      );
    }

    loads = true;

    const data = await response.json();
    const message = data.message;
    console.log(message);

    if (message) {
      loads = false;
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      onSubmit(donne);
    } else {
      console.error("Erreur");
      error = true;
      loads = false;
      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };

  let users = "";
  const getPosts = async () => {
    users = sessionStorage.getItem("identifiant");

    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/afficheDocument/" + users
    );

    const data = await res.json();
    const filter = data;

    // let r = filter.split("/");
    // r.shift();
    // let z = r.join("/");

    return filter;
  };

  onMount(async () => {
    // @ts-ignore
    if (update) {
      let post = await getPosts();
      console.log(post);
      donne.nom = post["nom"];
      donne.prenom = post["prenom"];
      donne.adresse = post["adresse"];
      donne.num = post["numCni"];
      // donne.photo = post["photo"];
      // donne.resid = post["certificat"];
      // donne.acte = post["acte"];
    }
  });
</script>

{#if success}
  <div
    class="alert alert-success"
    role="alert"
    style="position: fixed; bottom: 0; right: 20px"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-check-circle-fill"
      viewBox="0 0 16 16"
      style="margin-right: 10px;"
    >
      <path
        d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0m-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z"
      />
    </svg>
    Votre formulaire est envoyé avec success
  </div>
{/if}

{#if error}
  <div
    class="alert alert-danger d-flex align-items-center"
    role="alert"
    style="position: fixed; bottom: 0; right: 20px"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-exclamation-circle-fill"
      viewBox="0 0 16 16"
      style="margin-right: 10px;"
    >
      <path
        d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M8 4a.905.905 0 0 0-.9.995l.35 3.507a.552.552 0 0 0 1.1 0l.35-3.507A.905.905 0 0 0 8 4m.002 6a1 1 0 1 0 0 2 1 1 0 0 0 0-2"
      />
    </svg>
    <div>Erreur de remplissage du formulaire</div>
  </div>
{/if}

<center>
  <form>
    <div class="forms">
      <div class="input-container">
        <input
          required
          id="input"
          type="text"
          bind:value={donne.nom}
          on:change={(e) => {
            donne.nom = e.target.value;
          }}
        />
        <label class="label" for="input">nom</label>
        <div class="underline"></div>
      </div>
      <div class="input-container">
        <input
          required
          id="input"
          type="text"
          bind:value={donne.prenom}
          on:change={(e) => {
            donne.prenom = e.target.value;
          }}
        />
        <label class="label" for="input">prenom</label>
        <div class="underline"></div>
      </div>
      <div class="input-container">
        <input
          required
          id="input"
          type="text"
          bind:value={donne.adresse}
          on:change={(e) => {
            donne.adresse = e.target.value;
          }}
        />
        <label class="label" for="input">adresse</label>
        <div class="underline"></div>
      </div>

      <div class="input-container">
        <input
          required
          id="input"
          type="text"
          bind:value={donne.num}
          on:change={(e) => {
            donne.num = e.target.value;
          }}
        />
        <label class="label" for="input">Numero CNI</label>
        <div class="underline"></div>
      </div>

      <div class="input-container form-control-lg">
        <div class="mb-5">
          <label for="formFile" class="form-label">Photo d'identité</label>
          <input
            class="form-control"
            type="file"
            required
            accept="image/* "
            id="formFile"
            bind:value={donne.photo}
            on:change={(e) => {
              donne.photo = e.target.files[0];
            }}
          />
        </div>
        <div class="mb-5">
          <label for="formFile" class="form-label">Declaration de perte</label>
          <input
            class="form-control"
            type="file"
            required
            accept="image/* "
            id="formFile"
            bind:value={donne.perte}
            on:change={(e) => {
              donne.perte = e.target.files[0];
            }}
          />
        </div>

        <div class="mb-5">
          <label for="formFile" class="form-label"
            >Cértificat de résidence</label
          >
          <input
            class="form-control"
            type="file"
            required
            id="formFile"
            accept="image/* "
            bind:value={donne.resid}
            on:change={(e) => {
              donne.resid = e.target.files[0];
            }}
          />
        </div>

        {#if loads}
          <div class="three-body">
            <div class="three-body__dot"></div>
            <div class="three-body__dot"></div>
            <div class="three-body__dot"></div>
          </div>
        {/if}
      </div>
    </div>
    {#if !loads}
      <div class="input-container">
        <Motion let:motion whileHover={{ rotate: "3deg" }}>
          <input
            type="button"
            value="Suivant"
            class="button"
            use:motion
            on:click={handleSubmit}
          />
        </Motion>
      </div>
    {/if}
  </form>
</center>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  * {
    font-family: "Poppins", "Helvetica";
  }
  .input-container {
    position: relative;
    margin: 50px auto;
    width: 350px;
  }

  .input-container input[type="text"],
  .input-container input[type="password"] {
    font-size: 20px;
    width: 100%;
    border: none;
    border-bottom: 2px solid #000000;
    padding: 5px 0;
    background-color: transparent;
    outline: none;
  }

  .input-container .label {
    position: absolute;
    top: 0;
    left: 0;
    color: #000000;
    transition: all 0.3s ease;
    pointer-events: none;
  }

  .input-container input[type="text"]:focus ~ .label,
  .input-container input[type="text"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    font-weight: bold;
    color: #333;
  }

  .input-container input[type="password"]:focus ~ .label,
  .input-container input[type="password"]:valid ~ .label {
    top: -20px;
    font-size: 16px;
    color: #333;

    font-weight: bold;
  }

  .input-container .underline {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    width: 100%;
    background-color: #333;
    transform: scaleX(0);
    transition: all 0.3s ease;
  }

  .input-container input[type="text"]:focus ~ .underline,
  .input-container input[type="text"]:valid ~ .underline,
  .input-container input[type="password"]:focus ~ .underline,
  .input-container input[type="password"]:valid ~ .underline {
    transform: scaleX(1);
  }
  .button {
    width: 100%;
    height: auto;
    padding: 10px 0;
    font-size: 1.3em;
    font-weight: bold;
    background-color: white;
    border: 4px solid #5f6468;
    color: #5f6468;
    border-radius: 3px 20px 20px 3px;
    transition: 0.2s all;
  }
  .button:hover {
    background-color: #5f6468;
    color: white;
  }

  .three-body {
    --uib-size: 35px;
    --uib-speed: 0.8s;
    --uib-color: #000000;
    position: relative;
    display: inline-block;
    height: var(--uib-size);
    width: var(--uib-size);
    animation: spin78236 calc(var(--uib-speed) * 2.5) infinite linear;
  }

  .three-body__dot {
    position: absolute;
    height: 100%;
    width: 30%;
  }

  .three-body__dot:after {
    content: "";
    position: absolute;
    height: 0%;
    width: 100%;
    padding-bottom: 100%;
    background-color: var(--uib-color);
    border-radius: 50%;
  }

  .three-body__dot:nth-child(1) {
    bottom: 5%;
    left: 0;
    transform: rotate(60deg);
    transform-origin: 50% 85%;
  }

  .three-body__dot:nth-child(1)::after {
    bottom: 0;
    left: 0;
    animation: wobble1 var(--uib-speed) infinite ease-in-out;
    animation-delay: calc(var(--uib-speed) * -0.3);
  }

  .three-body__dot:nth-child(2) {
    bottom: 5%;
    right: 0;
    transform: rotate(-60deg);
    transform-origin: 50% 85%;
  }

  .three-body__dot:nth-child(2)::after {
    bottom: 0;
    left: 0;
    animation: wobble1 var(--uib-speed) infinite calc(var(--uib-speed) * -0.15)
      ease-in-out;
  }

  .three-body__dot:nth-child(3) {
    bottom: -5%;
    left: 0;
    transform: translateX(116.666%);
  }

  .three-body__dot:nth-child(3)::after {
    top: 0;
    left: 0;
    animation: wobble2 var(--uib-speed) infinite ease-in-out;
  }

  @keyframes spin78236 {
    0% {
      transform: rotate(0deg);
    }

    100% {
      transform: rotate(360deg);
    }
  }

  @keyframes wobble1 {
    0%,
    100% {
      transform: translateY(0%) scale(1);
      opacity: 1;
    }

    50% {
      transform: translateY(-66%) scale(0.65);
      opacity: 0.8;
    }
  }

  @keyframes wobble2 {
    0%,
    100% {
      transform: translateY(0%) scale(1);
      opacity: 1;
    }

    50% {
      transform: translateY(66%) scale(0.65);
      opacity: 0.8;
    }
  }
</style>
