<script>
	import imageCompression from 'browser-image-compression';
	let compressedFile;
	let progress = 0;
	let quality = 1;
	let selectedFile;
	let uploadProgress = 0;
	let showDialog = false;
	let isUploading = false;
	let isCompressing = false;

	async function handleImageUpload() {
		if (!selectedFile) return;
		showDialog = true;
		isCompressing = true;
		const options = {
			useWebWorker: true,
			onProgress: (number) => (progress = number),
			initialQuality: quality,
			alwaysKeepResolution: true
		};
		try {
			compressedFile = await imageCompression(selectedFile, options);
			progress = 100;
			setTimeout(() => {
				showDialog = false;
				isCompressing = false;
			}, 1000);
		} catch (error) {
			console.error(error);
			showDialog = false;
			isCompressing = false;
		}
	}

	function handleFileChange(e) {
		selectedFile = e.target.files[0];
		showDialog = true;
		isUploading = true;
		uploadProgress = 0;
		const interval = setInterval(() => {
			uploadProgress += 10;
			if (uploadProgress >= 100) {
				clearInterval(interval);
				setTimeout(() => {
					showDialog = false;
					isUploading = false;
				}, 1000);
			}
		}, 100);
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

<section class=" pb-20">
	<div class="w-full flex flex-col justify-center items-center">
		<h1 class="text-white text-center text-6xl font-semibold mt-20">Simple Compressor</h1>
		<p class=" text-gray-200 text-center mt-7 text-bs">
			Reduce the file size of your files with our Simple Compressor. Compress files quickly and
			easily.
		</p>

		<div class="mt-8 flex flex-col items-center gap-4">
			<div class="flex gap-4">
				<button on:click={handleImageUpload} class="px-6 py-2 bg-sky-500 grid mx-auto my-auto rounded-lg">Compress</button>
				<button on:click={downloadImage} class="px-6 py-2 bg-sky-500 grid mx-auto my-auto rounded-lg">Download</button>
			</div>
			<div class="w-full max-w-xs">
				<label for="quality-slider" class="block text-white text-sm mb-2 text-center">Image Quality: {quality}</label>
				<input id="quality-slider" type="range" bind:value={quality} min="0.1" max="1" step="0.1" class="w-full h-2 cursor-pointer" />
			</div>
		</div>
    
		<div
			class="w-5/12 h-72 border-dashed border-2 border border-[#27272a] grid mx-auto rounded-xl mt-10 p-8"
		>
			<img class="lucide lucide-cloud-upload flex mx-auto" src="/upload.svg" alt="upload" />
			<h1 class="text-white text-center relative bottom-2 text-xl">
				Drag your files here to compress
			</h1>
			<span class=" text-center text-white my-2">Or</span>
			<button class="px-6 py-2 bg-sky-500 grid mx-auto my-auto rounded-lg" on:click={openFileDialog}
				>Choose file</button
			>
			<input
				id="fileInput"
				type="file"
				accept="image/*"
				on:change={handleFileChange}
				style="display: none;"
			/>
		</div>
	</div>

	{#if showDialog}
		<div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
			<div class="bg-[#121212] p-6 rounded-lg border border-[#27272a]">
				{#if isUploading}
					<p class="text-white">Uploading: {uploadProgress}%</p>
				{:else if isCompressing}
					<p class="text-white">Compressing: {progress}%</p>
				{:else}
					<p class="text-white">Completed!</p>
				{/if}
			</div>
		</div>
	{/if}
</section>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100..900&display=swap');

	section {
		font-family: 'Lexend', sans-serif;
	}
</style>
