<script>
  import { Motion } from "svelte-motion";
</script>

<div>
  <h1>Madagascar Identité</h1>
  <div class="separe"></div>
  <div class="colone">
    <div>
      <ul>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/SuperAdmin/HomeSuperAdmin" use:motion>Accueil</a>
          </Motion>
        </li>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/SuperAdmin/Utilisateur" use:motion>Utilisateur</a>
          </Motion>
        </li>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/SuperAdmin/ChefSuperAdmin" use:motion>Chef</a>
          </Motion>
        </li>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/SuperAdmin/ArrondissementSuperAdmin" use:motion
              >Localisation</a
            >
          </Motion>
        </li>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/SuperAdmin/Generer" use:motion>Generer</a>
          </Motion>
        </li>
        <li>
          <div class="trois"></div>
          <Motion let:motion whileHover={{ scale: 1.1 }}>
            <a href="/Chef/MenuAdmin" use:motion>Menu</a>
          </Motion>
        </li>
      </ul>
    </div>
    <div class="low">
      <h4>Telecharger Mada Identity</h4>
      <br />
      <Motion let:motion whileHover={{ scale: 1.1, rotate: "3deg" }}>
        <button use:motion>Télécharger l'appli</button></Motion
      >
    </div>
  </div>
  <p style="text-align: right;">Copyright © 2024 Madagascar Identité</p>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    color: white;
    background-color: #343a40;
    padding: 10px;

    font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
  }
  .colone {
    display: flex;
    justify-content: space-between;
  }
  .separe {
    width: 100%;
    height: 10px;
    border-radius: 30px;
    background-color: white;
  }
  .low {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #343a40;
    width: 40%;
  }
  button {
    width: 100%;
    border: 4px solid white;
  }
  li {
    list-style: none;
    font-size: 1.2em;
    display: flex;
    align-items: center;
    gap: 20px;
  }
  li .trois {
    width: 40px;
    height: 20px;
    border-radius: 10px;
    background-color: white;
  }
  @media only screen and (max-width: 768px) {
    .colone {
      flex-direction: column;
    }
    .low {
      width: 100%;
    }
  }
</style>
