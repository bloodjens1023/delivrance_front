<script>
  // @ts-nocheck

  // @ts-ignore
  import Footer from "../../../Components/Footer.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import { onMount } from "svelte";
  import StatPie from "../../../Components/StatPie.svelte";
  import { goto } from "$app/navigation";
  import Erreur from "../../../Components/Erreur.svelte";
  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import Chargement from "../../../Components/Chargement.svelte";
  import Rate from "../../../Components/Rate.svelte";

  function filter(a) {
    console.log("https://bloodjens.pythonanywhere.com/" + a);
    return "https://bloodjens.pythonanywhere.com/" + a;
  }
  const datass = {
    labels: ["inscrit", "non inscrit"],
    datasets: [
      {
        label: "Les personnes inscrit / non inscrit",
        data: [70, 30],
        backgroundColor: ["rgb(255, 99, 132)", "rgb(54, 162, 235)"],
        hoverOffset: 5,
      },
    ],
  };
  const data2 = {
    labels: ["Janvier", "Fevrier", "Mars", "Avril"],
    datasets: [
      {
        label: "",
        data: [70, 30, 10, 40],
        backgroundColor: "rgb(54, 162, 235)",
        hoverOffset: 10,
      },
    ],
  };

  let val = "";
  let post = [];
  let users = "";
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

  onMount(async () => {
    try {
      let id = sessionStorage.getItem("identifiant");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        post = await getPosts();
      }
    } catch (error) {
      goto("/Error");
    }
  });
</script>

<div>
  {#await getPosts()}
    <HeaderAttente men="active" />
    <br />
    <br />
    <br />

    <center
      style="height: 400px; display: flex; align-items: center; justify-content: center;"
    >
      <Chargement />
    </center>
    <Footer />
  {:then data}
    {#if users != undefined}
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
      <div
        style="display: flex; align-items: center; justify-content: center; flex-direction: column;"
      >
        <div class="info">
          <center> <h2>Information Personnel</h2></center>
          <br /><br />
          <div class="prev">
            <div class="photos">
              <img
                src={filter(data["photo"])}
                alt=""
                srcset=""
                class="photo"
                style=" max-width: 100%; /* Empêcher l'image de dépasser la largeur de son conteneur */
            height: auto; "
              />
            </div>
            <div class="prop">
              <p>Identifiant : {data["identifiant"]}</p>
              <p>Email : {data["email"]}</p>
              <p>Tel : {data["tel"]}</p>
              <div style="display: flex;">
                <p style="">Mots de passe :</p>
                <p style="overflow: hidden; width: 300px;">
                  {data["password"]}
                </p>
              </div>
            </div>
          </div>
          <br />
          <div class="bones">
            <a
              href="/Utilisateur/Update"
              class="btn btn-dark"
              style="height:50px; width:200px; margin-right: 20px;">Modifier</a
            >
            <button
              class="btn btn-outline-danger"
              style="height:50px; width:200px; margin-right: 20px;"
              on:click={() => {
                sessionStorage.removeItem("identifiant");
                goto("/Utilisateur/Connexion");
              }}>Déconnecter</button
            >
          </div>
        </div>
        <br />
        <br />
        <br />
        <div
          style="width: 100%; display: flex; align-items: center; justify-content: center;"
        >
          <Rate simple={true} />
        </div>
        <br />
        <br />
        <br />
      </div>
      <FooterAttenteUtilisateur />
    {:else}
      <Erreur />
    {/if}
  {/await}
</div>

<style>
  .prev {
    display: flex;
    justify-content: space-between;
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
  .photo {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    padding: 20px;
    border-radius: 20px;
  }
  .prop {
    width: 70%;
  }
  .bones {
    display: flex;
    justify-content: right;
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
    .photo {
      margin-bottom: 30px;
    }
    .bones {
      justify-content: center;
    }
  }
</style>
