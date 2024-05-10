<script>
  // @ts-nocheck

  import Header from "../Components/Header.svelte";
  import logo from "$lib/logos.png";
  import Service from "../Components/Service.svelte";
  import Separateur from "../Components/Separateur.svelte";
  import { scale } from "svelte/transition";
  import { onMount } from "svelte";
  import { LottiePlayer } from "@lottiefiles/svelte-lottie-player";
  import animationData from "$lib/lotties/animation1.json";
  import animation2 from "$lib/lotties/animation2.json";
  import { Motion } from "svelte-motion";
  import Footer from "../Components/Footer.svelte";

  let visible = false;
  onMount(() => {
    visible = true;
  });
</script>

<title>Mada Identité</title>
{#if visible}
  <div>
    <Header />
    <br />
    <div
      style="display: flex; align-items: center; justify-content: center;flex-direction: column;"
    >
      <div class="heading" transition:scale={{ duration: 1000 }}>
        <Motion let:motion whileHover={{ scale: 1.1, rotate: "3deg" }}>
          <img src={logo} alt="" use:motion />
        </Motion>

        <h2>Madagascar Identité</h2>
        <h3>
          Bienvenue sur le Service de Délivrance <br /> en Ligne de la Carte Nationale
          d'Identité à Madagascar
        </h3>
      </div>

      <br />
      <br />
      <div class="separateur"></div>
      <br />
      <br />
      <div class="sect">
        <div>
          <div>
            <h2 style="text-decoration: underline; font-weight: 800;">
              Pourquoi choisir notre service en ligne ?
            </h2>
          </div>
          <div
            style="display: flex;flex-direction: column; align-items: center; justify-content: center;"
          >
            <br /><br /><br />
            <Service />
          </div>
        </div>
      </div>
      <br />
      <br />
      <div class="separateur"></div>
      <br />
      <br />
      <div
        class="sect"
        on:play={() => {
          console.log("testststststtst");
        }}
      >
        <div>
          <div>
            <h2 style="text-decoration: underline; font-weight: 800;">
              Comment ça marche ?
            </h2>
          </div>
        </div>
      </div>
      <div class="heading" transition:blur={{ duration: 1000 }}>
        <h2>Inscrivez-vous</h2>
        <br /><br />
        <h3>
          Pour profiter de notre application il faut vous inscrire <br />ou si
          vous en avez dejà connectez-vous
        </h3>
        <br /><br />
        <div>
          <Motion let:motion whileHover={{ scale: 1.1, rotate: "3deg" }}>
            <a
              use:motion
              class="btn btn-light btn-lg btns"
              style=" border-radius: 0;width: 300px; border:3px solid white;"
              href="/Utilisateur/Inscription">S'inscrire</a
            >
          </Motion>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a
              use:motion
              class="btn btn-lg btns"
              style="border-radius: 0; color:white; border:3px solid white; width: 300px;"
              href="/Utilisateur/Connexion">Se Connecter</a
            >
          </Motion>
        </div>
      </div>
      <br /><br /><br />
      <Separateur />
      <br /><br /><br />
      <div class="element">
        <div
          style="display: flex; align-items: center; justify-content: center; flex-direction: column; width: 70%;"
        >
          <h2 class="text-center">Remplissez le formulaire <br /> en ligne</h2>
          <h5 class="text-center">
            Complétez votre demande de CNI en <br /> fournissant les
            informations requises de <br /> manière simple et intuitive
          </h5>
        </div>
        <a
          style="display: flex; align-items: center; justify-content: center;"
          href="/"
        >
          <LottiePlayer
            src={animationData}
            autoplay={true}
            loop={true}
            renderer="svg"
            background="transparent"
            height={400}
            width={400}
          />
        </a>
      </div>
      <br /><br /><br />
      <Separateur />
      <br /><br /><br />
      <div class="element">
        <a
          style="display: flex; align-items: center; justify-content: center;"
          href="/"
        >
          <LottiePlayer
            src={animation2}
            autoplay={true}
            loop={true}
            renderer="svg"
            background="transparent"
            height={400}
            width={400}
          />
        </a>
        <div
          style="display: flex; align-items: center; justify-content: center; flex-direction: column; width: 50%; margin-top:30px"
        >
          <h2 class="text-center">
            Utiliser l’application <br /> Mada Identity
          </h2>
          <br />
          <h5 class="text-center">
            Telecharger l’application Mada Identity afin <br /> d’avoir une
            carte National d’identité <br /> éléctronique dans votre téléphone.
          </h5>
          <br /><br />
          <Motion let:motion whileHover={{ scale: 1.2, rotate: "10deg" }}>
            <button class="btn btn-outline-dark" use:motion
              >Telecharger l'appli</button
            >
          </Motion>
        </div>
      </div>
      <br /><br /><br />
      <div class="separateur"></div>

      <div
        class="sect"
        on:play={() => {
          console.log("testststststtst");
        }}
      >
        <div>
          <div>
            <h2 style="text-decoration: underline; font-weight: 800;">Autre</h2>
          </div>
        </div>
      </div>
      <div class="heading" transition:blur={{ duration: 1000 }}>
        <h2>Je suis un Chef d'arrondissement</h2>
        <br /><br />
        <h3>
          Pour les personnes Administrateur et les chef de chaque arrondissement <br
          />
          entrer ici pour vous connecter ou vous inscrire
        </h3>
        <br /><br />
        <div>
          <Motion let:motion whileHover={{ scale: 1.1, rotate: "3deg" }}>
            <a
              href="/Chef/InscriptionAdmin"
              use:motion
              class="btn btn-light btn-lg btns"
              style=" border-radius: 0;width: 300px; border:3px solid white;"
              >S'inscrire</a
            >
          </Motion>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a
              use:motion
              class="btn btn-lg btns"
              style="border-radius: 0; color:white; border:3px solid white; width: 300px;"
              href="/Chef/ConnexionAdmin">Se Connecter</a
            >
          </Motion>
        </div>
      </div>
    </div>
    <div style="margin-top: 200px;"></div>
    <Footer />
  </div>
{/if}

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  .element {
    width: 95%;
    height: auto;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .heading {
    height: 100vh;
    padding-bottom: 10px;
    padding-top: 10px;
    width: 95%;
    background: #343a40;
    margin-bottom: 50px;
    color: white;

    text-align: center;
    font-size: 20px;
    font-weight: bold;
    border-radius: 3px 3px 50px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    transition: opacity 0.5s ease-in-out;
  }
  h2 {
    font-size: 35px;
    font-family: "Poppins", "Helvetica";
  }
  h3 {
    font-size: 20px;
    opacity: 0.6;
    font-family: "Poppins", "Helvetica";
  }

  img {
    position: relative;
    width: 200px;
  }

  .separateur {
    width: 95%;
    height: 30px;
    background-color: black;
    border-radius: 20px;
  }
  .sect {
    width: 90%;
    height: 100%;
    padding: 40px 0 50px 0;
  }

  @media only screen and (max-width: 768px) {
    h2 {
      font-size: 20px;
    }
    h3 {
      font-size: 14px;
      opacity: 0.6;
      font-family: "Poppins", "Helvetica";
    }

    img {
      position: relative;
      width: 100px;
    }
    .element {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
    }
  }
</style>
