<script>
  import logo from "$lib/logos.png";
  import { Motion } from "svelte-motion";
  export let inscrit = false;
</script>

<nav
  id="mainNav"
  class="navbar navbar-expand-md sticky-top navbar-shrink py-3 navbar-light"
>
  <div class="container">
    <a class="navbar-brand d-flex align-items-center" href="/"
      ><span
        ><img
          src={logo}
          alt=""
          style="width: 70px; margin-right: 20px; "
        /></span
      ><span>Madagascar Identité</span></a
    ><button
      class="navbar-toggler"
      data-bs-toggle="collapse"
      data-bs-target="#navcol-1"
      ><span class="visually-hidden">Toggle navigation</span><span
        class="navbar-toggler-icon"
      ></span></button
    >
    <div id="navcol-1" class="collapse navbar-collapse">
      <ul class="navbar-nav mx-auto">
        <li class="nav-item"></li>
        <li class="nav-item"></li>
        <li class="nav-item"></li>
        <li class="nav-item"></li>
        <li class="nav-item"></li>
      </ul>
      {#if !inscrit}
        <Motion
          let:motion
          whileHover={{ scale: 1.1, rotate: "3deg" }}
          whileTap={{ scale: 1 }}
        >
          <a
            class="btn btn-dark shadow"
            role="button"
            href="/Utilisateur/Inscription"
            use:motion>S'inscrire</a
          >
        </Motion>
        <Motion
          let:motion
          whileHover={{ scale: 1.1, rotate: "3deg" }}
          whileTap={{ scale: 1 }}
        >
          <a
            style="margin-left: 20px;"
            class="btn btn-outline-dark shadow"
            role="button"
            href="/Utilisateur/Connexion"
            use:motion>Se connecter</a
          >
        </Motion>
      {/if}
    </div>
  </div>
</nav>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
</style>
