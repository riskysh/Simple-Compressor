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

  function openFileDialog() {
    document.getElementById('fileInput').click();
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

<section class="">

  <div class="w-full">
    <h1 class="text-white text-center  text-6xl font-semibold mt-20">Simple Compressor</h1>
    <p class=" text-gray-200 text-center   mt-7  text-bs">
      Reduce the file size of your files with our Simple Compressor. Compress files quickly and easily.
    </p>
    <button class="text-white bg-red-700">Compress</button>
    <button class="text-white bg-red-700">Download</button>
    <div class="w-5/12 h-72 border-dashed border-2 border border-[#27272a] grid mx-auto rounded-xl mt-10 p-8">
     <img class="lucide lucide-cloud-upload flex mx-auto" src="/upload.svg" alt="upload">
      <h1 class="text-white text-center relative bottom-2 text-xl">Drag your files here to compress</h1>
      <span class=" text-center text-white my-2">Or</span>
      <button class="px-6 py-2 bg-sky-500 grid mx-auto my-auto  rounded-lg" on:click={openFileDialog}>Choose file</button>
      <input id="fileInput" type="file" accept="image/*" on:change={handleFileChange} style="display: none;" />
    </div>
  </div>

</section>

<style>

@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100..900&display=swap');

section {
  font-family: 'Lexend', sans-serif;
}

</style>