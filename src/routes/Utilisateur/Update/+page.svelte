<script>
  // @ts-nocheck

  // @ts-ignore
  import Footer from "../../../Components/Footer.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";

  let post = [];
  let users = "";
  let loads = false;
  let success = false;
  let error = false;
  let checker = false;
  let checker1 = false;
  let datas;
  const getPosts = async () => {
    users = sessionStorage.getItem("identifiant");

    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/afficheUtilisateur/" + users
    );

    const data = await res.json();
    const filter = data;

    // let r = filter.split("/");
    // r.shift();
    // let z = r.join("/");

    return filter;
  };
  let donne = {};

  const handleSubmit = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    if (checker1 == true) {
      formdata.append("photo", "");
    } else {
      formdata.append("photo", donne.photo);
    }
    if (checker == true) {
      formdata.append("password", "");
    } else {
      formdata.append("password", donne.password);
    }

    formdata.append("tel", donne.tel);
    formdata.append("email", donne.email);

    let response;
    try {
      let use = sessionStorage.getItem("identifiant");
      response = await fetch(
        "https://bloodjens.pythonanywhere.com/updateUtilisateur/" + use,
        {
          method: "POST",
          body: formdata,
        }
      );

      loads = true;

      datas = await response.json();
      const message = datas.message;

      console.log(message);

      if (message == true) {
        success = true;
        setTimeout(() => {
          success = false;
          loads = false;
        }, 1000);

        goto("/Utilisateur/Menu");
      } else {
        setTimeout(() => {
          error = true;
          loads = false;
        }, 1000);
        setTimeout(() => {
          error = false;
        }, 2000);
      }
    } catch (error) {
      goto("/Error");
    }
  };

  onMount(async () => {
    if (
      sessionStorage.getItem("identifiant") == undefined ||
      sessionStorage.getItem("identifiant") == ""
    ) {
      goto("/Error");
    }
    post = await getPosts();
    donne.password = "";
    donne.tel = post.tel;
    donne.email = post.email;
    donne.photo = "";
  });
</script>

{#if success}
  <div
    class="alert alert-success"
    role="alert"
    style="position: fixed; bottom: 0; left: 20px"
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
    style="position: fixed; bottom: 0; left: 20px"
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
    <div>{datas.info}</div>
  </div>
{/if}

<div>
  <HeaderAttente men="active" />
  <br />
  <br />
  <br />
  <div style="padding: 0px 30px 0px 100px;">
    <h1>Vos informations</h1>
    <hr />
  </div>
  <br />
  <br />

  {#await getPosts()}
    <Chargement />
  {:then data}
    <form
      style="display: flex; align-items: center; justify-content: center; flex-direction: column;"
    >
      <div
        style="display: flex; align-items: center; justify-content: center; flex-direction: column;"
      >
        <div class="info" style="width: 100%; padding: 100px;">
          <center> <h2>Information Personnel</h2></center>
          <br /><br />
          <div class="prev">
            {#if !checker1}
              <div class="inputBox">
                <input
                  type="file"
                  required=""
                  bind:value={donne.photo}
                  on:change={(e) => {
                    donne.photo = e.target.files[0];
                  }}
                />
                <span>Photo de profil :</span>
              </div>
            {/if}
            <!-- checkbox -->
            <div class="checkbox-wrapper">
              <input
                id="terms-checkbox-37"
                name="checkbox"
                type="checkbox"
                bind:checked={checker1}
              />
              <label class="terms-label" for="terms-checkbox-37">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 200 200"
                  class="checkbox-svg"
                >
                  <mask fill="white" id="path-1-inside-1_476_5-37">
                    <rect height="200" width="200"></rect>
                  </mask>
                  <rect
                    mask="url(#path-1-inside-1_476_5-37)"
                    stroke-width="40"
                    class="checkbox-box"
                    height="200"
                    width="200"
                  ></rect>
                  <path
                    stroke-width="15"
                    d="M52 111.018L76.9867 136L149 64"
                    class="checkbox-tick"
                  ></path>
                </svg>
                <span class="label-text">Utiliser l'ancien photo de profil</span
                >
              </label>
            </div>
            <!-- fin checkbox -->

            <div class="inputBox">
              <input
                placeholder="Votre Tel..."
                type="tel"
                maxlength="10"
                bind:value={donne.tel}
                on:change={(e) => {
                  donne.tel = e.target.value;
                }}
              />
              <span>Tel :</span>
            </div>

            <div class="inputBox">
              <input
                placeholder="Votre email..."
                type="email"
                bind:value={donne.email}
                on:change={(e) => {
                  donne.email = e.target.value;
                }}
              />
              <span>Tel :</span>
            </div>
            {#if !checker}
              <div class="inputBox">
                <input
                  placeholder="Nouveau mots de passe..."
                  type="text"
                  bind:value={donne.password}
                  on:change={(e) => {
                    donne.password = e.target.value;
                  }}
                />
                <span>Mots de passe :</span>
              </div>
            {/if}
            <!-- checkbox -->
            <div class="checkbox-wrapper">
              <input
                id="terms-checkbox-38"
                name="checkbox"
                type="checkbox"
                bind:checked={checker}
                on:change={(e) => {
                  donne.password = "";
                }}
              />
              <label class="terms-label" for="terms-checkbox-38">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 200 200"
                  class="checkbox-svg"
                >
                  <mask fill="white" id="path-1-inside-1_476_5-37">
                    <rect height="200" width="200"></rect>
                  </mask>
                  <rect
                    mask="url(#path-1-inside-1_476_5-37)"
                    stroke-width="40"
                    class="checkbox-box"
                    height="200"
                    width="200"
                  ></rect>
                  <path
                    stroke-width="15"
                    d="M52 111.018L76.9867 136L149 64"
                    class="checkbox-tick"
                  ></path>
                </svg>
                <span class="label-text">Utiliser l'ancien mots de passe</span>
              </label>
            </div>
            <!-- fin checkbox -->
          </div>
          <br />
          <div class="bones" style="width: 400px;">
            {#if loads}
              <div class="three-body">
                <div class="three-body__dot"></div>
                <div class="three-body__dot"></div>
                <div class="three-body__dot"></div>
              </div>
            {/if}
            {#if !loads}
              <button
                class="btn btn-dark"
                type="submit"
                style="height:50px; width:200px; margin-right: 20px;"
                on:click={handleSubmit}>Enregistrer</button
              >
              <a
                class="btn btn-outline-danger"
                style="height:50px; width:200px; margin-right: 20px;"
                href="/Utilisateur/Menu">Annuler</a
              >
            {/if}
          </div>
        </div>
        <br />
        <br />
        <br />
      </div>
    </form>
  {/await}
  <Footer />
</div>

<style>
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
  .inputBox {
    position: relative;
  }

  .inputBox input {
    padding: 10px 20px;
    outline: none;
    background: transparent;
    border-radius: 5px;
    color: #212121;
    border: 1px solid#212121;
    font-size: 1em;
    width: 300px;
  }

  .inputBox span {
    position: absolute;
    left: 0;
    font-size: 0.7em;
    transform: translateX(14px) translateY(-7.5px);
    padding: 0 6px 1px 5px;
    border-radius: 2px;
    background: #ffffff;
    letter-spacing: 1px;
    border: 1px solid #212121;
    color: #212121;
    font-weight: bold;
  }

  .prev {
    display: flex;
    justify-content: center;
    flex-direction: column;
    gap: 30px;
    align-items: center;
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
  .checkbox-wrapper input[type="checkbox"] {
    display: none;
  }

  .checkbox-wrapper .terms-label {
    cursor: pointer;
    display: flex;
    align-items: center;
  }

  .checkbox-wrapper .terms-label .label-text {
    margin-left: 10px;
  }

  .checkbox-wrapper .checkbox-svg {
    width: 30px;
    height: 30px;
  }

  .checkbox-wrapper .checkbox-box {
    fill: rgba(207, 205, 205, 0.425);
    stroke: #000000;
    stroke-dasharray: 800;
    stroke-dashoffset: 800;
    transition: stroke-dashoffset 0.6s ease-in;
  }

  .checkbox-wrapper .checkbox-tick {
    stroke: #000000;
    stroke-dasharray: 172;
    stroke-dashoffset: 172;
    transition: stroke-dashoffset 0.6s ease-in;
  }

  .checkbox-wrapper input[type="checkbox"]:checked + .terms-label .checkbox-box,
  .checkbox-wrapper
    input[type="checkbox"]:checked
    + .terms-label
    .checkbox-tick {
    stroke-dashoffset: 0;
  }

  .bones {
    display: flex;
    justify-content: center;
    padding: 20px;
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
    .bones {
      justify-content: center;
    }
  }
</style>
