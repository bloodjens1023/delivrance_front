<script>
  // @ts-nocheck

  import { Motion } from "svelte-motion";
  export let comment = [];
  let commentaire = "";
  import { onMount } from "svelte";
  import toast, { Toaster } from "svelte-french-toast";
  // export let success = false;
  let identifiant;
  export let id = "";
  export let details = true;
  onMount(() => {
    // Récupérer l'identifiant de l'utilisateur depuis sessionStorage
    identifiant = sessionStorage.getItem("identifiant");
  });

  async function fetchComm(id) {
    let formdata = new FormData();
    formdata.append("identifiant", sessionStorage.getItem("identifiant"));
    formdata.append("contenue", commentaire);
    let co = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_commenter/" + id,
        {
          method: "POST",
          body: formdata,
        }
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateur:", error);
    }
  }
</script>

<Toaster />
<div class="comm2">
  <div style="width: 100%; ">
    {#each comment as comments, i}
      {#if !details}
        {#if i < 3}
          <div class="comm">
            <div class="row" style="padding: 10px;">
              <div
                style="width: 30px; height: 30px; background-color: black; border-radius: 30px;"
                class="col-2"
              ></div>
              <div class="col-10" style="font-weight: bold;">
                <p>{comments.utilisateur}</p>
              </div>
            </div>
            <p style="word-wrap: break-word; margin-left: 30px;">
              {comments.contenu}
            </p>
          </div>
        {/if}
      {/if}
      {#if details}
        <div class="comm">
          <div class="row" style="padding: 10px;">
            <div
              style="width: 30px; height: 30px; background-color: black; border-radius: 30px;"
              class="col-2"
            ></div>
            <div class="col-10" style="font-weight: bold;">
              <p>{comments.utilisateur}</p>
            </div>
          </div>
          <p style="word-wrap: break-word; margin-left: 30px;">
            {comments.contenu}
          </p>
        </div>
      {/if}
    {/each}
  </div>
  <br />
  <section class="dots-container">
    <div class="dot"></div>
    <div class="dot"></div>
    <div class="dot"></div>
  </section>
</div>
<br />
<div
  style="display: flex; gap:10px; align-items: center; justify-content: center;"
>
  <input type="text" bind:value={commentaire} />
  <Motion let:motion whileHover={{ scale: 1.2 }} whileTap={{ scale: 1.1 }}>
    <button
      style="width: 100px; background-color: white; border: none; padding-top: 10px; padding-bottom: 10px; background-color: #e9ebee;"
      use:motion
      on:click={() => {
        if (commentaire != "") {
          let a = [];
          for (const iterator of comment) {
            a.push(iterator);
          }

          fetchComm(id);
          comment = a;
          commentaire = "";
          toast.success("Commentaire ajouté", {
            style: "font-size:15px; padding:10px",
            duration: 2000,
          });
        } else {
          toast.error("Erreur de l'envoie", {
            style: "font-size:15px; padding:10px",
            duration: 2000,
          });
        }
      }}
      ><svg
        xmlns="http://www.w3.org/2000/svg"
        width="32"
        height="32"
        fill="currentColor"
        class="bi bi-send"
        viewBox="0 0 16 16"
      >
        <path
          d="M15.854.146a.5.5 0 0 1 .11.54l-5.819 14.547a.75.75 0 0 1-1.329.124l-3.178-4.995L.643 7.184a.75.75 0 0 1 .124-1.33L15.314.037a.5.5 0 0 1 .54.11ZM6.636 10.07l2.761 4.338L14.13 2.576zm6.787-8.201L1.591 6.602l4.339 2.76z"
        />
      </svg></button
    >
  </Motion>
</div>

<style>
  input {
    width: 350px;
    font-size: 1.5em;
    padding: 10px;
    border-radius: 10px;
    border: 3px solid black;
  }
  .comm {
    width: 515px;
    height: 100%;
    background-color: #d5d7e1;
    padding: 10px;
    margin-top: 5px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
  }
  .comm2 {
    width: 540px;
    border: 2px solid black;
    padding: 10px;
    border-radius: 10px;
  }

  @media only screen and (max-width: 768px) {
    .comm {
      width: 300px;
    }
    .comm2 {
      width: 330px;
    }
    input {
      width: 250px;
      font-size: 1.5em;
      border-radius: 10px;
      border: 3px solid black;
    }
  }
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
