<script>
  // @ts-nocheck

  import { Motion } from "svelte-motion";
  import Formulaire from "../../../Components/Formulaire.svelte";
  import Header from "../../../Components/Header.svelte";
  import FormulaireDupliUse from "../../../Components/FormulaireDupliUse.svelte";
  import FormulaireDupliPerte from "../../../Components/FormulaireDupliPerte.svelte";
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  $: decision = 0;
  let submittedData = null;
  let current = "0";
  onMount(() => {
    try {
      let users = sessionStorage.getItem("identifiant");
      if (users == "" || users == null || users == undefined) {
        goto("error");
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  <Header inscrit="true" />
  <br />
  <center
    ><div
      class="progress"
      role="progressbar"
      aria-label="Success example"
      aria-valuenow="70"
      aria-valuemin="0"
      aria-valuemax="100"
      style="width: 70%; height: 30px;"
    >
      <div
        class="progress-bar bg-dark"
        style="width: {current * 50}%; height:90px"
      ></div>
    </div></center
  >
  {#if current == "0"}
    <h2 style="margin: 30px;">Quelle est la raison de votre présence ?</h2>
    <center>
      <div class="mb-5"></div>
      <div class="mb-5">
        <Motion let:motion whileHover={{ rotate: "5deg" }}>
          <button
            use:motion
            class="btn btn-dark btn-lg"
            style="width: 300px;"
            on:click={() => {
              decision = 0;
              current = 1;
            }}>Demande de CNI</button
          ></Motion
        >
      </div>
      <div class="mb-5">
        <Motion let:motion whileHover={{ rotate: "5deg" }}>
          <button
            class="btn btn-dark btn-lg"
            style="width: 300px;"
            use:motion
            on:click={() => {
              decision = 1;
              current = "1";
            }}>Mon CNI est usée</button
          >
        </Motion>
      </div>
      <div class="mb-5">
        <Motion let:motion whileHover={{ rotate: "5deg" }}
          ><button
            class="btn btn-dark btn-lg"
            style="width: 300px;"
            use:motion
            on:click={() => {
              decision = 2;
              current = "1";
            }}>J'ai perdu mon CNI</button
          ></Motion
        >
      </div>
    </center>
    <Motion let:motion whileHover={{ scale: 1.2 }}>
      <a
        href="/Utilisateur/Attente"
        use:motion
        style="position: absolute; left: 10px; bottom:30px;text-decoration: underline;"
        >Me demander plus tard</a
      >
    </Motion>
  {/if}

  {#if current == "1"}
    <h2 style="margin: 50px 0 0 100px">Remplissez les formulaires</h2>
    {#if decision == 0}
      <Formulaire
        onSubmit={(data) => {
          submittedData = data;
          console.log(data);
          current = "2";
        }}
      />
    {/if}

    <!-- decision -->
    {#if decision == 2}
      <FormulaireDupliPerte
        onSubmit={(data) => {
          submittedData = data;
          console.log(data);
          current = "2";
        }}
      />
    {/if}

    {#if decision == 1}
      <FormulaireDupliUse
        onSubmit={(data) => {
          submittedData = data;
          console.log(data);
          current = "2";
        }}
      />
    {/if}

    <Motion let:motion whileHover={{ scale: 1.1 }}>
      <button
        style="position:fixed; bottom:20px; left:20px;"
        use:motion
        class="return"
        on:click={() => {
          current = "0";
        }}>retour</button
      >
    </Motion>
  {/if}
  {#if current == "2"}
    <br />
    <br />
    <br />
    <center>
      <h2>Votre demande est en cours de <br /> traitement</h2>
      <div style="margin: 150px 0;"></div>
      <div id="page">
        <div id="container">
          <div id="ring"></div>
          <div id="ring"></div>
          <div id="ring"></div>
          <div id="ring"></div>
          <div id="h3">En attente</div>
        </div>
      </div>
      <Motion let:motion whileHover={{ scale: 1.1, rotate: "3deg" }}>
        <a
          style="margin-top: 110px;"
          class="btn btn-outline-dark shadow"
          role="button"
          href="/Utilisateur/Attente"
          use:motion>Retourner en attentes</a
        >
      </Motion>
    </center>
  {/if}
  <br /><br />
</div>

<style>
  @import url("$lib/bootstrap.min.css");
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  * {
    font-family: "Poppins", "Helvetica";
  }

  .return {
    width: 150px;
    height: 50px;
    font-size: 1.2em;
    border: none;
    border-radius: 30px;
  }

  #page {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #container {
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  #h3 {
    color: rgb(0, 0, 0);
    font-size: 1.3em;
    font-weight: bold;
  }

  #ring {
    width: 190px;
    height: 190px;
    border: 1px solid transparent;
    border-radius: 50%;
    position: absolute;
  }

  #ring:nth-child(1) {
    border-bottom: 8px solid #949295;
    animation: rotate1 2s linear infinite;
  }

  @keyframes rotate1 {
    from {
      transform: rotateX(50deg) rotateZ(110deg);
    }

    to {
      transform: rotateX(50deg) rotateZ(470deg);
    }
  }

  #ring:nth-child(2) {
    border-bottom: 8px solid rgb(255, 65, 106);
    animation: rotate2 2s linear infinite;
  }

  @keyframes rotate2 {
    from {
      transform: rotateX(20deg) rotateY(50deg) rotateZ(20deg);
    }

    to {
      transform: rotateX(20deg) rotateY(50deg) rotateZ(380deg);
    }
  }

  #ring:nth-child(3) {
    border-bottom: 8px solid #46a06b;
    animation: rotate3 2s linear infinite;
  }

  @keyframes rotate3 {
    from {
      transform: rotateX(40deg) rotateY(130deg) rotateZ(450deg);
    }

    to {
      transform: rotateX(40deg) rotateY(130deg) rotateZ(90deg);
    }
  }

  #ring:nth-child(4) {
    border-bottom: 8px solid #e7e4e4;
    animation: rotate4 2s linear infinite;
  }

  @keyframes rotate4 {
    from {
      transform: rotateX(70deg) rotateZ(270deg);
    }

    to {
      transform: rotateX(70deg) rotateZ(630deg);
    }
  }
</style>
