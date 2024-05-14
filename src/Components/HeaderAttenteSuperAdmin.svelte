<script>
  import logo from "$lib/logos.png";

  export let acc = "";
  export let dem = "";
  export let men = "";
  export let chef = "";
  export let gener = "";
  export let arr = "";
</script>

<nav
  class="navbar navbar-expand-md sticky-top navbar-shrink py-3 navbar-light"
  id="mainNav"
>
  <div class="container">
    <a
      class="navbar-brand d-flex align-items-center"
      href="/SuperAdmin/HomeSuperAdmin"
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
          <a class="nav-link {acc}" href="/SuperAdmin/HomeSuperAdmin">Accueil</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link {dem}" href="/SuperAdmin/Utilisateur"
            >Utilisateurs</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link {chef}" href="/SuperAdmin/ChefSuperAdmin">Chefs</a>
        </li>

        <li class="nav-item">
          <a class="nav-link {arr}" href="/SuperAdmin/ArrondissementSuperAdmin"
            >Localisation</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link {gener}" href="/SuperAdmin/Generer">generer</a>
        </li>
        <li class="nav-item">
          <a class="nav-link {men}" href="/SuperAdmin/MenuSuperAdmin">Menu</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<style>
  @import url("$lib/bootstrap.min.css");
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
</style>
