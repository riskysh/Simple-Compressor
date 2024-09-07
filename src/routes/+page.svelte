<script>
  import imageCompression from 'browser-image-compression';
  let compressedFile;

  async function handleImageUpload(event) {
    const file = event.target.files[0];
    if (!file) return;

    const options = {
      maxSizeMB: 1,
      maxWidthOrHeight: 1920,
      useWebWorker: true
    };

    try {
      compressedFile = await imageCompression(file, options);
    } catch (error) {
      console.error(error);
    }
  }

  function downloadImage() {
    if (!compressedFile) return;
    const url = URL.createObjectURL(compressedFile);
    const a = document.createElement('a');
    a.href = url;
    a.download = compressedFile.name;
    a.click();
    URL.revokeObjectURL(url);
  }
</script>

<svelte:head>
	<title>Simple Compressor</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>

	<h1 class="text-5xl font-bold">Simple Compressor</h1>

	<input type="file" accept="image/*" on:change={handleImageUpload} />

	<button class=" bg-stone-500" on:click={downloadImage}>Download Compressed Image</button>

</section>

<style>

</style>
