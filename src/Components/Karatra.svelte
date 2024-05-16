<script>
  // @ts-nocheck

  import kart from "$lib/drapeau.webp";
  import photo from "$lib/photo.jpg";
  import logo from "$lib/logos.png";

  import QRCode from "qrcode-generator";
  import { onMount } from "svelte";
  import { Motion } from "svelte-motion";
  import ChargementKaratra from "./ChargementKaratra.svelte";
  function filter(a) {
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  let qrData = "Jenny Fandresena \n test";
  let qrCodeUrl = "";
  let ok = false;
  function generateQRCode() {
    const qr = QRCode(0, "M");
    qr.addData(qrData);
    qr.make();
    qrCodeUrl = qr.createDataURL();
  }
  function insererEspace(str) {
    return str.replace(/\d{3}(?=\d)/g, "$& ");
  }

  onMount(() => {
    fetchCart();
  });

  async function fetchCart() {
    let id = sessionStorage.getItem("identifiant");

    let co = [];
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/affiche_cni/" + id
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      if (co.length > 0) {
        ok = true;
        qrData =
          "Nom : " +
          co[0]["nom"] +
          "\nPrenom : " +
          co[0]["prenom"] +
          "\nAdresse : " +
          co[0]["adresse"] +
          "\nNumero CNI :" +
          co[0]["numCNI"];
        generateQRCode();
        return co[0];
      } else {
        return false;
      }
    } catch (error) {
      console.error("Erreur lors de la récupération des Utilisateur:", error);
    }
  }
</script>

{#await fetchCart()}
  <ChargementKaratra />
{:then dats}
  {#if ok}
    <div>
      <div class="flip-card">
        <div class="flip-card-inner">
          <div class="flip-card-front">
            <p class="heading_8264">REPOBLIKAN'I MADAGASIKARA</p>
            <p class="heading_8265">Fitiavana - Tanindrazana - Fandrosoana</p>
            <p class="heading_8266">Anarana/Nom</p>
            <p class="heading_8267">{dats["nom"]}</p>
            <p class="heading_8268">Fanampiny/Prenom</p>
            <p class="heading_8269">{dats["prenom"]}</p>
            <p class="heading_8270">Laharana/n°</p>

            <p class="heading_8271 cni">{insererEspace(dats["numCNI"])}</p>

            <img src={kart} alt="" srcset="" class="chip" />
            <img
              src={filter(dats["photo"])}
              alt=""
              width="90px"
              class="photo"
            />
            <img src={logo} alt="" class="back" />
          </div>
          <div class="flip-card-back">
            <p class="heading_8272">Fonenana/Domicile</p>
            <p class="heading_8273">{dats["adresse"]}</p>
            <p class="heading_8274">Natao Tamin'ny/Fait le</p>
            <p class="heading_8275">{dats["date"]}</p>
            <div class="strip"></div>
            <img src={logo} alt="" class="back2" />
            <div class="code">
              <img src={qrCodeUrl} alt="QR Code" style="width: 70px;" />
            </div>
          </div>
        </div>
      </div>
    </div>
  {/if}
  {#if !ok}
    <center
      style="height: 70vh; display: flex; align-items: center; justify-content: center; flex-direction: column; gap:30px"
    >
      <div class="loader"></div>
      <div class="load"><h1>Aucun virtualisation identifier</h1></div>
      <br />
    </center>
  {/if}
{/await}

<style>
  .flip-card {
    background-color: transparent;
    width: 350px;
    height: 200px;
    perspective: 1000px;
    color: rgb(0, 0, 0);
  }

  .heading_8264 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 2.5em;
    left: 13em;
  }
  .heading_8265 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 4em;
    left: 10em;
  }
  .heading_8266 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.4em;
    font-weight: bold;
    top: 10em;
    left: 22em;
  }
  .heading_8267 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 9.4em;
    left: 17em;
  }
  .heading_8268 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.4em;
    font-weight: bold;
    top: 15em;
    left: 22em;
  }
  .heading_8269 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 13.5em;
    left: 17em;
  }
  .heading_8270 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.4em;
    font-weight: bold;
    top: 20em;
    left: 22em;
  }
  .heading_8271 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 17.5em;
    left: 17em;
  }
  .heading_8272 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.4em;
    font-weight: bold;
    top: 3em;
    left: 7em;
  }
  .heading_8273 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 4em;
    left: 5em;
  }
  .heading_8274 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.4em;
    font-weight: bold;
    top: 7.5em;
    left: 7em;
  }
  .heading_8275 {
    position: absolute;
    letter-spacing: 0.2em;
    font-size: 0.5em;
    font-weight: bold;
    top: 7.5em;
    left: 5em;
  }

  .chip {
    position: absolute;
    top: 1em;
    left: 1em;
    width: 30px;
  }
  .photo {
    position: absolute;
    top: 4em;
    left: 1em;
    width: 90px;
    border-radius: 10px;
  }
  .back {
    position: absolute;
    top: 3em;
    left: 7em;
    opacity: 0.3;
    width: 150px;
    border-radius: 10px;
  }
  .back2 {
    position: absolute;
    top: 1em;
    right: 1em;
    width: 50px;
    border-radius: 10px;
  }

  .strip {
    position: absolute;
    background-color: black;
    width: 15em;
    height: 1.5em;
    border-radius: 0px 20px 20px 0px;
    top: 9.6em;
    background: repeating-linear-gradient(
      45deg,
      #303030,
      #303030 10px,
      #202020 10px,
      #202020 20px
    );
  }

  .code {
    font-weight: bold;
    text-align: center;
    margin: 0.2em;
    color: black;
  }

  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
  }

  .flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
  }

  .flip-card-front,
  .flip-card-back {
    box-shadow: 0 8px 14px 0 rgba(0, 0, 0, 0.2);
    position: absolute;
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    border-radius: 1rem;
  }

  .flip-card-front {
    box-shadow:
      rgba(0, 0, 0, 0.4) 0px 2px 2px,
      rgba(0, 0, 0, 0.3) 0px 7px 13px -3px,
      rgba(0, 0, 0, 0.2) 0px -1px 0px inset;
    background-color: #ffffff;
  }

  .flip-card-back {
    box-shadow:
      rgba(0, 0, 0, 0.4) 0px 2px 2px,
      rgba(0, 0, 0, 0.3) 0px 7px 13px -3px,
      rgba(0, 0, 0, 0.2) 0px -1px 0px inset;
    background-color: #ffffff;
    transform: rotateY(180deg);
  }
  .code {
    position: absolute;
    bottom: 1.2em;
    right: 1em;
    border-radius: 2.5px;
  }
  .loader {
    width: fit-content;
    font-weight: bold;
    font-family: monospace;
    font-size: 30px;
    background: radial-gradient(circle closest-side, #000 94%, #0000)
      right/calc(200% - 1em) 100%;
    animation: l24 3s infinite alternate linear;
  }

  .loader::before {
    content: "Aucun Demande identifier";
    line-height: 1em;
    color: #0000;
    background: inherit;
    background-image: radial-gradient(circle closest-side, #fff 94%, #000);
    -webkit-background-clip: text;
    background-clip: text;
  }
  .load {
    display: none;
  }

  @keyframes l24 {
    100% {
      background-position: left;
    }
  }

  @media only screen and (max-width: 768px) {
    .loader {
      display: none;
    }
    .load {
      display: block;
    }
  }
</style>
