<script>
  // @ts-nocheck

  let donne = {};

  const verif_certificat = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    formdata.append("certificat", donne.certificat);
    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_verif_certificat/",
      {
        method: "POST",
        body: formdata,
      }
    );
    const data = await response.json();
    let users = data.data;
    console.log(users);
  };
  const verif_photo = async (event) => {
    event.preventDefault();
    let formdata = new FormData();
    formdata.append("photo", donne.photo);
    let response;
    response = await fetch(
      "https://bloodjens.pythonanywhere.com/api_verif_photo/",
      {
        method: "POST",
        body: formdata,
      }
    );
    const data = await response.json();
    let users = data.data;
    console.log(users);
  };
</script>

<div
  style="display: flex; align-items: center; justify-content: center; width: 50%;"
>
  <input
    class="form-control"
    type="file"
    required
    accept="image/* "
    id="formFile"
    bind:value={donne.photo}
    on:change={(e) => {
      donne.photo = e.target.files[0];
    }}
  />
  <button on:click={verif_photo}>tester</button>

  <br /><br />
  <input
    class="form-control"
    type="file"
    required
    accept="image/* "
    id="formFile"
    bind:value={donne.certificat}
    on:change={(e) => {
      donne.certificat = e.target.files[0];
    }}
  />
  <button on:click={verif_certificat}>tester</button>
</div>
