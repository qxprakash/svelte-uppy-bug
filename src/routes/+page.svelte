<script lang="ts">
import Uppy from "@uppy/core";
import "@uppy/core/dist/style.css";
import "@uppy/dashboard/dist/style.css";
import Tus from "@uppy/tus";
import { Dashboard } from "@uppy/svelte";
import { onMount } from "svelte";

const uppy = new Uppy({
	debug: true,
	autoProceed: false,
	restrictions: {
		maxFileSize: 10000000000000,
		maxNumberOfFiles: 3,
		minNumberOfFiles: 1,
		allowedFileTypes: ["image/*", "video/*"],
	},
});

uppy.on("complete", (result) => {
	console.log("Upload complete! Files:", result.successful);
});

onMount(() => {
	uppy.use(Tus, { endpoint: "https://tusd.tusdemo.net/files/" });

	return () => {
		uppy.destroy();
	};
});
</script>

<main class="p-8 max-w-4xl mx-auto">
  <h1 class="text-4xl font-bold mb-8 text-center">Uppy Dashboard Example</h1>

  <div class="mb-6">
    <p class="text-lg text-gray-600 text-center">
      A minimal Svelte app showcasing the Uppy Dashboard component for file uploads
    </p>
  </div>

  <Dashboard
    {uppy}
    props={{
      width: 750,
      height: 550,
      showProgressDetails: true,
      proudlyDisplayPoweredByUppy: false,
      note: 'Images and videos only, 1MB max file size, up to 3 files'
    }}
  />
</main>

<style>
  :global(.uppy-Dashboard) {
    font-family: inherit;
    border: 1px solid #e5e7eb;
    border-radius: 0.5rem;
  }
</style>