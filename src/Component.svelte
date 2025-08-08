<script>
  import { getContext, onMount } from "svelte";

  export let url = ""; // Budibase setting: PDF URL

  const { styleable } = getContext("sdk");
  const component = getContext("component");
  let pdfContainer;
  let error = "";

  $: pdfUrl = url;

  onMount(async () => {
    if (!url) return;
    error = "";
    // Load pdf.js from CDN if not already loaded
    if (!window.pdfjsLib) {
      await new Promise((resolve, reject) => {
        const script = document.createElement("script");
        script.src = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.2.67/pdf.min.js";
        script.onload = resolve;
        script.onerror = reject;
        document.head.appendChild(script);
      });
    }
    if (!window.pdfjsLib) {
      error = "Failed to load PDF.js library.";
      return;
    }
    window.pdfjsLib.GlobalWorkerOptions.workerSrc = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.2.67/pdf.worker.min.js";
    try {
      const pdf = await window.pdfjsLib.getDocument(url).promise;
      const page = await pdf.getPage(1);
      const viewport = page.getViewport({ scale: 1.5 });
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      canvas.width = viewport.width;
      canvas.height = viewport.height;
      pdfContainer.innerHTML = "";
      pdfContainer.appendChild(canvas);
      await page.render({ canvasContext: context, viewport }).promise;
    } catch (e) {
      error = "Failed to load PDF: " + e.message;
    }
  });
</script>

<div use:styleable={$component.styles}>
  <div><strong>PDF URL:</strong> {pdfUrl}</div>
  {#if error}
    <div style="color: red">{error}</div>
  {/if}
  <div bind:this={pdfContainer} style="border:1px solid #ccc; margin-top:1em; min-height: 400px;"></div>
</div>
