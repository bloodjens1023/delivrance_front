<script>
  // @ts-nocheck

  import logo from "$lib/logos.png";
  import { onMount, onDestroy } from "svelte";

  import Comment from "./Comment.svelte";
  import Like from "./Like.svelte";
  import { Motion } from "svelte-motion";
  import { goto } from "$app/navigation";
  export let description = "";
  export let aimer = 0;
  export let image = "";
  export let ids = 0;
  export let comm = [];
  export let detail = false;

  async function fetchComm(id) {
    let co = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_get_comment/" + id
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      comm = co;
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateur:", error);
    }
  }
  onMount(() => {
    fetchComm(ids);
  });
  let u = setInterval(() => {
    fetchComm(ids);
  }, 500);
  onDestroy(() => {
    clearInterval(u);
  });
</script>

<div>
  <div class="card">
    <div class="content">
      <div style="display: flex; align-items: center; gap:10px">
        <img src={logo} alt="" style="width: 70px; " />
        <span style="font-size: 1.5em; font-weight: bold;">Administrateur</span>
      </div>
      <p class="para">
        {description}
      </p>
      <center style="width: 100%;">
        <img id="img" src={image} alt="" style="" />
      </center>
      <div
        style="display: flex; align-items: center; justify-content: space-around; width:100%"
      >
        <Like count={aimer} id={ids} />
        <button
          class="bookmarkBtn"
          on:click={() => {
            sessionStorage.setItem("pub_id", ids);
            goto("/Utilisateur/Betail");
          }}
        >
          <span class="IconContainer">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              style="fill: white"
              class="bi bi-plus-circle-fill"
              viewBox="0 0 16 16"
            >
              <path
                d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M8.5 4.5a.5.5 0 0 0-1 0v3h-3a.5.5 0 0 0 0 1h3v3a.5.5 0 0 0 1 0v-3h3a.5.5 0 0 0 0-1h-3z"
              />
            </svg>
          </span>
          <p class="text">Voir plus</p>
        </button>
      </div>
    </div>
  </div>
  <br />
  <br />
  <div
    style="display: flex; align-items: center; justify-content: center;flex-direction: column;"
  >
    <Comment comment={comm} id={ids} details={detail} />
  </div>
</div>

<style>
  .card {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 600px;
    border-radius: 24px;
    line-height: 1.6;
    transition: all 0.48s cubic-bezier(0.23, 1, 0.32, 1);
  }
  #img {
    height: 250px;
    margin-top: 40px;
    margin-bottom: 30px;
    border-radius: 10px;
  }
  .content {
    display: flex;
    flex-direction: column;
    width: calc(100% - 50px);
    align-items: flex-start;
    gap: 24px;
    padding: 36px;
    border-radius: 0px 0px 30px 30px;
    color: #000000;
    overflow: hidden;
    background: #ffffff;
    border: 3px solid #000000;
    transition: all 0.48s cubic-bezier(0.23, 1, 0.32, 1);
  }
  .bookmarkBtn {
    width: 170px;
    height: 40px;
    border-radius: 40px;
    border: none;
    background-color: rgb(255, 255, 255);
    display: flex;
    align-items: baseline;
    justify-content: center;
    cursor: pointer;
    transition-duration: 0.3s;
    overflow: hidden;
    box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.062);
  }

  .IconContainer {
    width: 37px;
    height: 37px;
    background-color: rgb(0, 0, 0);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    z-index: 2;
    transition-duration: 0.3s;
  }

  .text {
    height: 100%;
    width: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: rgb(26, 26, 26);
    z-index: 1;
    transition-duration: 0.3s;
    font-size: 1.04em;
    font-weight: 600;
  }

  .bookmarkBtn:hover .IconContainer {
    width: 180px;
    border-radius: 40px;
    transition-duration: 0.3s;
  }

  .bookmarkBtn:hover .text {
    transform: translate(100px);
    width: 0;
    font-size: 0;
    transition-duration: 0.3s;
  }

  .bookmarkBtn:active {
    transform: scale(0.95);
    transition-duration: 0.3s;
  }
  @media only screen and (max-width: 768px) {
    .card {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 450px;
      border-radius: 24px;
      line-height: 1.6;
      transition: all 0.48s cubic-bezier(0.23, 1, 0.32, 1);
    }
    #img {
      width: 300px;
      height: auto;
    }
  }
</style>
