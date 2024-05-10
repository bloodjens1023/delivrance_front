<script>
  import { M, Motion } from "svelte-motion";
</script>

<div class="row row-cols-1 row-cols-md-2 d-flex justify-content-center">
  <Motion let:motion whileHover={{ scale: 1.04 }} transition={{ times: [1] }}>
    <div class="col mb-4" use:motion>
      <div class="card bg-dark" style="height: 100%;">
        <div class="card-body text-center px-4 py-5 px-md-5">
          <p class="fw-bold text-light card-text mb-2">Rapidité</p>
          <h5 class=" card-title mb-3" style="color: rgba(255, 255, 255, 0.6);">
            Obtenez votre CNI plus rapidement que jamais grâce à notre processus
            en ligne efficace
          </h5>
          <br />
          <br />
          <Motion let:motion whileHover={{ scale: 1.1, rotate: "5deg" }}>
            <button class="btn btn-light" type="button" use:motion
              >savoir plus</button
            ></Motion
          >
        </div>
      </div>
    </div>
  </Motion>
  <Motion let:motion whileHover={{ scale: 1.04 }} transition={{ times: [1] }}>
    <div class="col mb-4" use:motion>
      <div class="card bg-light" style="height: 100%;">
        <div class="card-body text-center px-4 py-5 px-md-5">
          <p class="fw-bold text-dark card-text mb-2">Confort</p>
          <h5 class=" card-title mb-3" style="color: rgba(0, 0, 0, 0.801);">
            Évitez les déplacements et les files d'attente interminables en
            faisant votre demande de CNI depuis chez vous, à tout moment qui
            vous convient
          </h5>
          <br />
          <Motion let:motion whileHover={{ scale: 1.1, rotate: "5deg" }}>
            <button class="btn btn-dark" type="button" use:motion
              >savoir plus</button
            >
          </Motion>
        </div>
      </div>
    </div>
  </Motion>
  <Motion let:motion whileHover={{ scale: 1.04 }} transition={{ times: [1] }}>
    <div class="col mb-4" use:motion>
      <div class="card bg-dark" style="height: 100%;">
        <div class="card-body text-center px-4 py-5 px-md-5">
          <p class="fw-bold text-light card-text mb-2">Sécurité</p>
          <h5 class=" card-title mb-3" style="color: rgba(255, 255, 255, 0.6);">
            Nous accordons une importance primordiale à la sécurité de vos
            données personnelles. Vos informations sont traitées de manière
            sécurisée et confidentielle
          </h5>
          <br />
          <Motion let:motion whileHover={{ scale: 1.1, rotate: "5deg" }}>
            <button class="btn btn-light" type="button" use:motion
              >savoir plus</button
            ></Motion
          >
        </div>
      </div>
    </div>
  </Motion>
</div>

<style>
  @import "$lib/bootstrap.min.css";
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");

  * {
    font-family: "Poppins", "Helvetica";
  }
</style>
