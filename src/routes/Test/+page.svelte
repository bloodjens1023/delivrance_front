<script>
  // @ts-nocheck

  let apiKey = "acc_e201826d4119173";
  let apiSecret = "5aec3dbd2376f2f590ac3f7f3e3df044";
  let result = "";
  let imageFile;

  const handleFileChange = async (event) => {
    try {
      const files = event.target.files;
      if (files.length === 0) {
        return;
      }

      imageFile = files[0];

      // Create FormData and append the image file
      const formData = new FormData();
      formData.append("image", imageFile);

      // Make the API request
      const apiResponse = await fetch("https://api.imagga.com/v2/text", {
        method: "POST",
        headers: {
          Authorization: "Basic " + btoa(apiKey + ":" + apiSecret),
        },
        body: formData,
      });

      const jsonResponse = await apiResponse.json();
      const textData = jsonResponse.result.text;

      for (let j of textData) {
        if (j.data.toLowerCase() === "certificat de residence") {
          result = "ok";
          break;
        } else {
          result = "not found";
        }
      }
    } catch (error) {
      console.error("Error:", error);
      result = "error";
    }
  };
</script>

<main>
  <h1>Upload an Image</h1>
  <input type="file" accept="image/*" on:change={handleFileChange} />
  <h2>Result: {result}</h2>
</main>
