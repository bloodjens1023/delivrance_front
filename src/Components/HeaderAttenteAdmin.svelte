<script>
  import logo from "$lib/logos.png";
  import { onDestroy, onMount } from "svelte";
  import { Motion } from "svelte-motion";
  export let acc = "";
  export let dem = "";
  export let men = "";
  export let notif = "";

  let co = [];
  let count = 0;
  async function fetchDemande() {
    let id = sessionStorage.getItem("chef");

    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_liste_demande_arrond/" + id
      );
      const data = await response.json();
      co = data.data; // Supposons que 'data' est le nom de la clé qui contient les Utilidateur dans la réponse JSON
      count = co.length;
    } catch (error) {
      console.error("Erreur lors de la récupération des demandes:", error);
    }
  }

  onMount(() => {
    fetchDemande();
  });

  let u = setInterval(() => {
    fetchDemande();
  }, 2000);

  onDestroy(() => {
    clearInterval(u);
  });
</script>

<nav
  class="navbar navbar-expand-md sticky-top navbar-shrink py-3 navbar-light"
  id="mainNav"
>
  <div class="container">
    <a class="navbar-brand d-flex align-items-center" href="/Chef/AccueilAdmin"
      ><span><img src={logo} alt="" style="width: 70px; " /></span></a
    ><button
      data-bs-toggle="collapse"
      class="navbar-toggler"
      data-bs-target="#navcol-1"
      ><span class="visually-hidden">Toggle navigation</span><span
        class="navbar-toggler-icon"
      ></span></button
    >
    <div class="collapse navbar-collapse" id="navcol-1">
      <ul class="navbar-nav mx-auto">
        <li class="nav-item">
          <a class="nav-link {acc}" href="/Chef/AccueilAdmin">Accueil</a>
        </li>
        <li class="nav-item">
          <a
            class="nav-link {dem}"
            href="/Chef/DemandeAdmin"
            data-after-type="badge top right"
            data-after-text={count}>Demande</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link {notif}" href="/Chef/Recuperer">recuperation</a>
        </li>
        <li class="nav-item">
          <a class="nav-link {men}" href="/Chef/MenuAdmin">Menu</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<style>
  @import url("$lib/bootstrap.min.css");
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  .notification {
    margin-left: 5px;
    background-color: black;
    padding: 5px;
    border-radius: 10px;
    color: white;
  }
  [data-after-text] {
    --badge-offset-x: calc(0px - var(--badge-size) / 3);
    --badge-offset-y: calc(0px - var(--badge-size) / 3);
    --badge-size: 1.5rem;
    --circle-size: 2rem;
    --dot-offset: 0.5rem;
    --dot-size: 0.5rem;

    --b: initial;
    --bgc: hsl(0, 0%, 0%);
    --bdrs: 0;
    --c: hsl(195, 100%, 99%);
    --d: inline-flex;
    --fz: 0.625rem;
    --fw: 700;
    --h: auto;
    --l: initial;
    --m: 0.4rem;
    --p: 0;
    --pos: static;
    --r: initial;
    --t: initial;
    --tt: uppercase;
    --w: initial;

    position: relative;
  }

  [data-after-text]:not([data-after-text=""])::after {
    content: attr(data-after-text);
  }

  [data-after-text]:not([data-after-text=""])::after {
    align-items: center;
    background: var(--bgc);
    border-radius: var(--bdrs);
    bottom: var(--b);
    box-shadow: var(--bxsh);
    box-sizing: border-box;
    color: var(--c);
    display: var(--d);
    font-size: var(--fz);
    font-weight: var(--fw);
    height: var(--h);
    justify-content: center;
    left: var(--l);
    padding: var(--p);
    position: var(--pos);
    right: var(--r);
    text-decoration: none;
    text-transform: var(--tt);
    top: var(--t);
    width: var(--w);
  }

  /* MODIFIERS */
  [data-after-type*="badge"]::after {
    --bdrs: var(--badge-size);
    --bxsh: 0 0 0 2px rgba(255, 255, 255, 0.5);
    --h: var(--badge-size);
    --p: 0;
    --pos: absolute;
    --w: var(--badge-size);
  }

  /* POSITION */
  [data-after-type*="top"]::after {
    --b: auto;
    --pos: absolute;
    --t: var(--dot-offset);
  }
  [data-after-type*="right"]::after {
    --l: auto;
    --pos: absolute;
    --r: var(--dot-offset);
  }

  [data-after-type*="badge"][data-after-type*="top"]::after {
    --m: 0;
    --t: var(--badge-offset-y);
  }
  [data-after-type*="badge"][data-after-type*="right"]::after {
    --m: 0;
    --r: var(--badge-offset-x);
  }
</style>
