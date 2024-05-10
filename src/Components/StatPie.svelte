<script>
  // @ts-nocheck

  import { goto } from "$app/navigation";
  import Chart from "chart.js/auto";
  import { onMount } from "svelte";
  export let type = "pie";
  export let nom = "myChart";

  function creer(dataa) {
    const ctx = document.getElementById(nom);
    let datass = {
      labels: ["encour", "refuser", "accepter"],
      datasets: [
        {
          label: "Statitique",
          data: dataa,
          backgroundColor: [
            "rgb(255, 99, 132)",
            "rgb(54, 162, 235)",
            "rgb(255, 205, 86)",
          ],
          hoverOffset: 5,
        },
      ],
    };
    new Chart(ctx, {
      type: type,
      data: datass,
    });
  }
  async function stat() {
    try {
      let id = sessionStorage.getItem("chef");

      if (id == null || id == undefined || id == "") {
        goto("/Error");
      } else {
        try {
          const response = await fetch(
            "https://bloodjens.pythonanywhere.com/api_stat_arrondissement_chef/" +
              id
          );
          const data = await response.json();
          const co = data.data;
          creer([co[0]["encour"], co[0]["refuser"], co[0]["accepter"]]);
        } catch (error) {
          console.error("Erreur lors de la récupération des demandes:", error);
        }
      }
    } catch (error) {
      goto("/Error");
    }
  }
  onMount(() => {
    stat();
  });
</script>

<canvas id={nom} style="width: 100%;"></canvas>

<style></style>
