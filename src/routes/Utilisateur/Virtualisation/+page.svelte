<script>
  import FooterAttenteUtilisateur from "../../../Components/FooterAttenteUtilisateur.svelte";
  import HeaderAttente from "../../../Components/HeaderAttente.svelte";
  import Karatra from "../../../Components/Karatra.svelte";
</script>

<HeaderAttente virt="active" />
<div
  style="display: flex; align-items: center; justify-content: center; height: 70vh;"
>
  <Karatra />
</div>

<FooterAttenteUtilisateur />
