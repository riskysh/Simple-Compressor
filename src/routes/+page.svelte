<script>
  import imageCompression from 'browser-image-compression';
  let compressedFile;
  let progress = 0;
  let quality = 1;
  let selectedFile;
  let uploadProgress = 0;

  async function handleImageUpload() {
    if (!selectedFile) return;

    const options = {
      useWebWorker: true,
      onProgress: (number) => progress = number,
      initialQuality: quality,
      alwaysKeepResolution: true
    };

    try {
      compressedFile = await imageCompression(selectedFile, options);
      progress = 100;
    } catch (error) {
      console.error(error);
    }
  }

  function handleFileChange(e) {
    selectedFile = e.target.files[0];
    uploadProgress = 100;
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

<section class="  text-emerald-300 h-screen bg-emerald-700">

	<h1 class="text-5xl font-bold">Simple Compressor</h1>
  <h2 class="text-4xl font-bold">upload progress {uploadProgress}%</h2>
  <h2 class="text-4xl font-bold">compressor progress {progress}%</h2>

	<input type="file" accept="image/*" multiple on:change={handleFileChange} />
  <h2 class="text-4xl font-bold">quality {quality}</h2>
  <input type="range" min="0.1" max="1" step="0.1" bind:value={quality} />
  <button on:click={handleImageUpload} class=" bg-stone-600">compress</button>
	<button class=" bg-stone-500" on:click={downloadImage}>Download Compressed Image</button>

</section>
