<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import PubList from "./PubList.svelte";
  import { Motion } from "svelte-motion";

  let ids = 0;
  let image = "";
  let description = "";
  let error = false;
  let success = false;
  let loads = false;
  let actu = false;
  let modifier = false;

  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  const handleSubmit = async (event) => {
    event.preventDefault();
    loads = true;
    let formdata = new FormData();
    formdata.append("photo", image);
    formdata.append("description", description);

    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_ajout_pub/",
      {
        method: "POST",
        body: formdata,
      }
    );

    const data = await response.json();
    const message = data.message;

    if (message) {
      console.log("pub inserer");
      loads = false;
      success = true;
      description = "";
      image = "";
      fetchUtilisateur();
      window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
      setTimeout(() => {
        success = false;
      }, 1000);
    } else {
      error = true;
      loads = false;

      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };

  const handleChange = async (id) => {
    loads = true;
    let formdata = new FormData();
    formdata.append("photo", image);
    formdata.append("description", description);

    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_modif_pub/" + id,
      {
        method: "POST",
        body: formdata,
      }
    );

    const data = await response.json();
    const message = data.message;
    console.log(message);

    if (message) {
      console.log("pub modifier");
      loads = false;
      success = true;
      description = "";
      image = "";
      fetchUtilisateur();
      window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
      modifier = false;
      setTimeout(() => {
        success = false;
      }, 1000);
    } else {
      error = true;
      loads = false;

      setTimeout(() => {
        error = false;
      }, 1000);
    }
  };

  const getPosts = async (user) => {
    const res = await fetch(
      "https://bloodjens.pythonanywhere.com/api_delete_pub/" + user,
      {
        method: "POST",
      }
    );

    const data = await res.json();
    const filter = data;
    if (filter) {
      users = [];
      fetchUtilisateur();
      success = true;
      setTimeout(() => {
        success = false;
      }, 1000);
      return true;
    } else {
      return false;
    }
  };

  let users = [];
  let filtrer = users;
  async function fetchUtilisateur() {
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_affiche_pub/"
      );
      const data = await response.json();
      users = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON

      filtrer = users;
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateur:", error);
    }
  }

  onMount(() => {
    fetchUtilisateur();
  });
  let recherche = "";
  function search() {
    filtrer = [];
    for (const i of users) {
      if (i["description"].toLowerCase().includes(recherche.toLowerCase())) {
        filtrer.push(i);
      }
    }
    console.log(filtrer);
  }
</script>

{#if success}
  <div
    class="alert alert-success"
    role="alert"
    style="position: fixed; bottom: 0; right: 20px, width:auto ;z-index: 1; display:flex; align-item:center;justify-content:center;"
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
    <span> Votre publication à été ajouter dans l'actualité </span>
  </div>
{/if}

{#if error}
  <div
    class="alert alert-danger d-flex align-items-center"
    role="alert"
    style="position: fixed; bottom: 0; right: 20px; z-index: 1;width:auto ;"
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
<div class="cards" id="cad">
  {#if !modifier}
    <span class="titles" style="text-decoration: underline;"
      >créer un publication</span
    >
  {/if}
  {#if modifier}
    <span class="titles" style="text-decoration: underline;"
      >modifier un publication</span
    >
  {/if}
  <br /><br />
  <form class="forms">
    <center>
      <div class="file-upload-forms">
        <label for="file" class="file-upload-labels">
          <div class="file-upload-designs">
            <svg viewBox="0 0 640 512" height="1em">
              <path
                d="M144 480C64.5 480 0 415.5 0 336c0-62.8 40.2-116.2 96.2-135.9c-.1-2.7-.2-5.4-.2-8.1c0-88.4 71.6-160 160-160c59.3 0 111 32.2 138.7 80.2C409.9 102 428.3 96 448 96c53 0 96 43 96 96c0 12.2-2.3 23.8-6.4 34.6C596 238.4 640 290.1 640 352c0 70.7-57.3 128-128 128H144zm79-217c-9.4 9.4-9.4 24.6 0 33.9s24.6 9.4 33.9 0l39-39V392c0 13.3 10.7 24 24 24s24-10.7 24-24V257.9l39 39c9.4 9.4 24.6 9.4 33.9 0s9.4-24.6 0-33.9l-80-80c-9.4-9.4-24.6-9.4-33.9 0l-80 80z"
              ></path>
            </svg>
            <p>Glisser et Déposer</p>
            <p>ou</p>
            <span class="browse-buttons">Image de la publication</span>
          </div>
          <input
            id="file"
            type="file"
            bind:files={image}
            on:change={(e) => {
              image = e.target.files[0];
            }}
          />
        </label>
      </div>
    </center>
    <br /><br />
    <div class="groups">
      <textarea
        placeholder=""
        id="comment"
        name="comment"
        rows="5"
        bind:value={description}
        on:change={(e) => {
          description = e.target.value;
        }}
      ></textarea>
      <label for="comment">Déscription</label>
    </div>

    <br />
    {#if !modifier}
      <button
        on:click={(e) => {
          if (description == "") {
            error = true;
            setTimeout(() => {
              error = false;
            }, 1000);
          } else {
            handleSubmit(e);
          }
        }}>Publier</button
      >
    {/if}
    {#if modifier}
      <button
        on:click={(e) => {
          handleChange(ids);
          image = "";
          description = "";
        }}>Modifier</button
      >
      <br />
      <button
        style="background-color: crimson;"
        on:click={(e) => {
          image = "";
          description = "";
          modifier = false;
        }}>Annuler</button
      >
    {/if}
  </form>
</div>
<br /><br /><br />
<!-- code de recherche -->
<center>
  <form class="form">
    <label for="search" class="label">
      <input
        class="input"
        type="text"
        required=""
        bind:value={recherche}
        on:input={() => {
          search();
        }}
        placeholder="Rechercher un identifiant ..."
        id="search"
      />
      <div class="fancy-bg"></div>
      <div class="search">
        <svg
          viewBox="0 0 24 24"
          aria-hidden="true"
          class="svg r-14j79pv r-4qtqp9 r-yyyyoo r-1xvli5t r-dnmrzs r-4wgw6l r-f727ji r-bnwqim r-1plcrui r-lrvibr"
        >
          <g>
            <path
              d="M21.53 20.47l-3.66-3.66C19.195 15.24 20 13.214 20 11c0-4.97-4.03-9-9-9s-9 4.03-9 9 4.03 9 9 9c2.215 0 4.24-.804 5.808-2.13l3.66 3.66c.147.146.34.22.53.22s.385-.073.53-.22c.295-.293.295-.767.002-1.06zM3.5 11c0-4.135 3.365-7.5 7.5-7.5s7.5 3.365 7.5 7.5-3.365 7.5-7.5 7.5-7.5-3.365-7.5-7.5z"
            ></path>
          </g>
        </svg>
      </div>
      <button
        class="close-btn"
        type="reset"
        on:click={() => {
          filtrer = users;
        }}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 svg"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
            clip-rule="evenodd"
          ></path>
        </svg>
      </button>
    </label>
  </form>
  <br /><br />
</center>
<!-- fin code de recherche -->
<center>
  <h2>Liste des publications</h2>
</center>
<br />
<table>
  <thead>
    <tr>
      <th scope="col">Identifiant</th>
      <th scope="col">photo</th>
      <th scope="col">description</th>

      <th scope="col">J'aime</th>
      <th scope="col">Modifier</th>
      <th scope="col">Supprimer</th>
    </tr>
  </thead>

  <tbody>
    {#each filtrer as user, i}
      <tr>
        <td data-label="Id">{i + 1}</td>
        <td data-label="Photo">
          <img src={filter(user["photo"])} alt="" width="150px" />
        </td>
        <td data-label="Déscription">{user["description"]}</td>
        <td data-label="Aimer">{user["aimer"]} 💗</td>
        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-warning"
              use:motion
              on:click={() => {
                image = "";
                description = user["description"];
                ids = user["id"];
                let cad = document.getElementById("cad");
                window.scrollTo({
                  top: cad.scrollTop,
                  behavior: "smooth", // Pour un défilement en douceur
                });
                modifier = true;
              }}
              ><svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-pencil"
                viewBox="0 0 16 16"
              >
                <path
                  d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325"
                />
              </svg></button
            ></Motion
          ></td
        >
        <td>
          <Motion let:motion whileHover={{ rotate: "5deg", scale: 1.1 }}>
            <button
              class="btn btn-danger"
              use:motion
              on:click={() => {
                getPosts(user["id"]);
              }}
              ><svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-trash3"
                viewBox="0 0 16 16"
              >
                <path
                  d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5M11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47M8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5"
                />
              </svg></button
            ></Motion
          ></td
        >
      </tr>
    {/each}
  </tbody>
</table>
<br />
{#if users.length == 0}
  <p>Aucun publication identifier</p>
{/if}
<br /><br /><br />

<style>
  .cards {
    background-color: #fff;
    border: 3px solid black;
    border-radius: 10px;

    padding: 20px;
    width: 600px;
    display: flex;
    flex-direction: column;
  }

  .titles {
    font-size: 40px;
    font-weight: bold;
    text-align: center;
    font-variant: small-caps;
  }

  .forms {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
  }

  .groups {
    position: relative;
  }

  .forms .groups label {
    font-size: 14px;
    color: rgb(99, 102, 102);
    position: absolute;
    top: -10px;
    left: 10px;
    background-color: #fff;
    transition: all 0.3s ease;
  }

  .forms .groups textarea {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    margin-bottom: 20px;
    outline: 0;
    width: 100%;
    background-color: transparent;
  }

  .forms .groups textarea:placeholder-shown + label {
    top: 10px;
    background-color: transparent;
  }

  .forms .groups textarea:focus {
    border-color: #000000;
  }

  .forms .groups textarea:focus + label {
    top: -10px;
    left: 10px;
    background-color: #fff;
    color: #000000;
    font-weight: 600;
    font-size: 14px;
  }

  .forms .groups textarea {
    resize: none;
    height: 200px;
  }

  .forms button {
    background-color: #000000;
    color: #fff;
    border: none;
    border-radius: 30px;
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .forms button:hover {
    background-color: #303030;
  }

  .file-upload-forms {
    width: fit-content;
    height: fit-content;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .file-upload-labels input {
    display: none;
  }
  .file-upload-labels svg {
    height: 50px;
    fill: rgb(0, 0, 0);
    margin-bottom: 20px;
  }
  .file-upload-labels {
    cursor: pointer;
    background-color: #ffffff;
    padding: 30px 70px;
    border-radius: 40px;
    border: 2px dashed rgb(0, 0, 0);
  }
  .file-upload-designs {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 5px;
  }
  .browse-buttons {
    background-color: rgb(0, 0, 0);
    padding: 5px 15px;
    border-radius: 10px;
    color: white;
    transition: all 0.3s;
  }
  .browse-buttons:hover {
    background-color: rgb(14, 14, 14);
  }

  table {
    border: 1px solid #ccc;
    border-collapse: collapse;
    margin: 0;
    padding: 0;
    width: 90%;
    table-layout: fixed;
  }
  table tr {
    background-color: #f8f8f8;
    border: 1px solid #ddd;
    padding: 0.35em;
  }

  table th,
  table td {
    padding: 0.625em;
    text-align: center;
  }

  table th {
    font-size: 0.85em;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  /* debut css de code de rejcercje */
  .form {
    --input-text-color: #fff;
    --input-bg-color: #283542;
    --focus-input-bg-color: transparent;
    --text-color: #949faa;
    --active-color: #1b9bee;
    --width-of-input: 600px;
    --inline-padding-of-input: 1.2em;
    --gap: 0.9rem;
  }
  /* form style */
  .form {
    font-size: 0.9rem;
    display: flex;
    gap: 0.5rem;
    align-items: center;
    width: var(--width-of-input);
    position: relative;
    isolation: isolate;
  }
  /* a fancy bg for showing background and border when focus. */
  .fancy-bg {
    position: absolute;
    width: 100%;
    inset: 0;
    background: var(--input-bg-color);
    border-radius: 30px;
    height: 100%;
    z-index: -1;
    pointer-events: none;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  }
  /* label styling */
  .label {
    width: 100%;
    padding: 0.8em;
    height: 40px;
    padding-inline: var(--inline-padding-of-input);
    display: flex;
    align-items: center;
  }

  .search,
  .close-btn {
    position: absolute;
  }
  /* styling search-icon */
  .search {
    fill: var(--text-color);
    left: var(--inline-padding-of-input);
  }
  /* svg -- size */
  .svg {
    width: 17px;
    display: block;
  }
  /* styling of close button */
  .close-btn {
    border: none;
    right: var(--inline-padding-of-input);
    box-sizing: border-box;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    padding: 0.1em;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--active-color);
    opacity: 0;
    visibility: hidden;
  }
  /* styling of input */
  .input {
    color: var(--input-text-color);
    width: 100%;
    margin-inline: min(2em, calc(var(--inline-padding-of-input) + var(--gap)));
    background: none;
    border: none;
  }

  .input:focus {
    outline: none;
    color: black;
  }

  .input::placeholder {
    color: var(--text-color);
  }
  /* input background change in focus */
  .input:focus ~ .fancy-bg {
    border: 1px solid var(--active-color);
    background: var(--focus-input-bg-color);
  }
  /* search icon color change in focus */
  .input:focus ~ .search {
    fill: var(--active-color);
  }
  /* showing close button when typing */
  .input:valid ~ .close-btn {
    opacity: 1;
    visibility: visible;
  }
  /* this is for the default background in input,when selecting autofill options -- you can remove this code if you do not want to override the browser style.  */
  input:-webkit-autofill,
  input:-webkit-autofill:hover,
  input:-webkit-autofill:focus,
  input:-webkit-autofill:active {
    -webkit-transition: "color 9999s ease-out, background-color 9999s ease-out";
    -webkit-transition-delay: 9999s;
  }
  /* fin code de recherche */
  @media screen and (max-width: 600px) {
    table {
      border: 0;
    }

    table thead {
      border: none;
      clip: rect(0 0 0 0);
      height: 1px;
      margin: -1px;
      overflow: hidden;
      padding: 0;
      position: absolute;
      width: 1px;
    }

    table tr {
      border-bottom: 3px solid #ddd;
      display: block;
      margin-bottom: 0.625em;
    }

    table td {
      border-bottom: 1px solid #ddd;
      display: block;
      font-size: 0.8em;
      text-align: right;
    }

    table td::before {
      /*
        * aria-label has no advantage, it won't be read inside a table
        content: attr(aria-label);
        */
      content: attr(data-label);
      float: left;
      font-weight: bold;
      text-transform: uppercase;
    }

    table td:last-child {
      border-bottom: 0;
    }
  }
</style>
