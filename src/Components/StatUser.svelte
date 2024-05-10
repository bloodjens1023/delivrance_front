<script>
  // @ts-nocheck
  import Chart from "chart.js/auto";
  import { onMount } from "svelte";
  let datass = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
  let graph;
  let anne;
  let ctx;

  let datas = {
    labels: [
      "Janvier",
      "Fevrier",
      "Mars",
      "Avril",
      "Mai",
      "Juin",
      "Juillet",
      "Aout",
      "Septembre",
      "Octobre",
      "Novembre",
      "Decembre",
    ],
    datasets: [
      {
        label: "Utilisateur inscrit",
        data: datass,
        backgroundColor: [
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
          "white",
        ],
        borderColor: "rgb(75, 192, 192)",
        tension: 0.4,
      },
    ],
  };

  onMount(() => {
    ctx = document.getElementById("graph").getContext("2d");
    graph = new Chart(ctx, {
      type: "line",
      data: datas,
      options: {
        scales: {
          y: {
            beginAtZero: true,
          },
        },
      },
    });
  });
  function creer(data) {
    datas.datasets[0].data = data;
    graph.update();
  }

  async function loadstat() {
    let id = anne;
    try {
      const response = await fetch(
        "https://bloodjens.pythonanywhere.com/api_stat_utilisateur_arrond_tous/" +
          id
      );
      const data = await response.json();
      const co = data.data;
      console.log(co);
      creer([
        co[0]["Janvier"],
        co[1]["Fevrier"],
        co[2]["Mars"],
        co[3]["Avril"],
        co[4]["Mai"],
        co[5]["Juin"],
        co[6]["Juillet"],
        co[7]["Aout"],
        co[8]["Septembre"],
        co[9]["Octobre"],
        co[10]["Novembre"],
        co[11]["Decembre"],
      ]);
    } catch (error) {
      console.error("Erreur lors de la récupération des demandes:", error);
    }
  }
</script>

<div class="can">
  <canvas id="graph"></canvas>
  <br />
  <div style="display: flex; align-items: center; justify-content: center;">
    <select
      class="form-select"
      aria-label=" select example"
      on:change={(e) => {
        anne = e.target.value;
        loadstat();
      }}
    >
      <option selected>Année</option>
      <option value="2024">2024</option>
      <option value="2025">2025</option>
      <option value="2026">2026</option>
      <option value="2027">2027</option>
    </select>
  </div>
</div>

<style>
  .can {
    width: 100%;
  }
  select {
    width: auto;
  }
</style>
