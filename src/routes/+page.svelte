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

<section class="">

  <div class="w-full">
    <h1 class="text-white text-left ml-28 text-6xl font-semibold mt-20">Simple Compressor</h1>
    <p class="text-white text-left ml-28 mt-2 text-xl">
      Reduce the file size of your files with our Simple Compressor. Compress files quickly and easily.
    </p>
    <div class="w-10/12 h-72 border border-[#27272a] grid mx-auto rounded-xl mt-10">
      <svg xmlns="http://www.w3.org/2000/svg" width="96" height="96" viewBox="0 0 24 24" fill="none" stroke="#ffffff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-cloud-upload flex mx-auto mt-10"><path d="M12 13v8"/><path d="M4 14.899A7 7 0 1 1 15.71 8h1.79a4.5 4.5 0 0 1 2.5 8.242"/><path d="m8 17 4-4 4 4"/></svg>
      <h1 class="text-white text-center relative bottom-2 text-xl">Put your files here to compress</h1>
      <button class="px-6 py-2 bg-sky-500 grid mx-auto my-auto mb-12 rounded-lg">Choose files</button>
    </div>
  </div>

</section>

<style>

@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100..900&display=swap');

section {
  font-family: 'Lexend', sans-serif;
}

</style>