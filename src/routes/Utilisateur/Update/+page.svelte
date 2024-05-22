<script>
  // @ts-nocheck

  // @ts-ignore
  import Footer from "../../../Components/Footer.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import Chargement from "../../../Components/Chargement.svelte";
  import FooterAttenteSuperAdmin from "../../../Components/FooterAttenteSuperAdmin.svelte";
  import ChargementConnect from "../../../Components/ChargementConnect.svelte";
  import toast, { Toaster } from "svelte-french-toast";

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
    loads = true;
    try {
      let use = sessionStorage.getItem("identifiant");
      response = await fetch(
        "https://bloodjens.pythonanywhere.com/updateUtilisateur/" + use,
        {
          method: "POST",
          body: formdata,
        }
      );

      datas = await response.json();
      const message = datas.message;

      console.log(message);

      if (message == true) {
        toast.success("Mise a jour réussite", {
          style: "font-size:20px; padding:10px",
        });

        goto("/Utilisateur/Menu");
      } else {
        loads = false;
        toast.error("Erreur de la mise à jour", {
          style: "font-size:20px; padding:10px",
        });
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

<Toaster />
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
    <form class="forms">
      <div
        style="display: flex; align-items: center; justify-content: center; flex-direction: column;"
      >
        <div class="info">
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
          <div class="bones" style="width: 100%;">
            {#if loads}
              <center>
                <ChargementConnect />
              </center>
            {/if}
            {#if !loads}
              <center>
                <button
                  class="btn btn-dark"
                  type="submit"
                  style="height:50px; width:200px; margin-right: 20px; margin-top:10px"
                  on:click={handleSubmit}>Enregistrer</button
                >
                <a
                  class="btn btn-outline-danger"
                  style="height:50px; width:200px; margin-right: 20px; margin-top:10px"
                  href="/Utilisateur/Menu">Annuler</a
                >
              </center>
            {/if}
          </div>
        </div>
        <br />
        <br />
        <br />
      </div>
    </form>
  {/await}
  <FooterAttenteSuperAdmin />
</div>

<style>
  .forms {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
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
  h2 {
    font-size: 1.7rem;
    font-weight: bold;
    text-decoration: underline;
  }
  .info {
    width: 700px;
    border: 3px solid black;
    border-radius: 3px 3px 20px 20px;
    padding-top: 30px;
    padding-bottom: 30px;
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
    h1 {
      font-size: 1.7rem;
    }
    .info {
      width: 90%;
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
