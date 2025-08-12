<script>
// @ts-nocheck
import { onDestroy, onMount } from "svelte";

export let src = ""; // string URL or File/Blob
export let width = "100%";
export let height = "600px";
export let showDownload = true; // show download button

let objectSrc = null;
let isBlob = false;
let isClient = false;

// Bind URL methods
const createObjectURL = (...args) => URL.createObjectURL.apply(URL, args);
const revokeObjectURL = (...args) => URL.revokeObjectURL.apply(URL, args);

onMount(() => {
  isClient = true; // mark that we're in browser
});

// Reactive to changes in src, but only process blobs in client
$: {
  if (isBlob && objectSrc) {
    revokeObjectURL(objectSrc);
    objectSrc = null;
    isBlob = false;
  }

  if (!src) {
    objectSrc = null;
    isBlob = false;
  } else if (typeof src === "string") {
    objectSrc = src;
    isBlob = false;
  } else if (isClient && (src instanceof Blob || (typeof File !== "undefined" && src instanceof File))) {
    objectSrc = createObjectURL(src);
    isBlob = true;
  } else {
    // unsupported type
    if (src) console.warn("PdfViewer: unsupported src type", src);
    objectSrc = null;
    isBlob = false;
  }
}

onDestroy(() => {
  if (isBlob && objectSrc) {
    revokeObjectURL(objectSrc);
  }
});

function openPdfInNewTab() {
  if (objectSrc && isClient) {
    window.open.call(window, objectSrc, "_blank");
  }
}

let fallbackText =
  "Your browser does not support embedding PDFs. You can download the file instead.";
</script>

<style>
/* same styles as before */
</style>

<div
  class="pdf-wrapper"
  style="--pdf-width: {width}; --pdf-height: {height};"
>
  <div class="controls">
    {#if objectSrc}
      <button on:click={openPdfInNewTab}>Open in new tab</button>
      {#if showDownload}
        <a href={objectSrc} download class="link" style="text-decoration:none">
          <button>Download</button>
        </a>
      {/if}
    {:else}
      <button disabled>Open in new tab</button>
      <button disabled>Download</button>
    {/if}
    <div class="note">
      Rendered via browser PDF viewer (no libraries)
    </div>
  </div>

  {#if objectSrc}
    <!-- svelte-ignore a11y_missing_attribute -->
    <!-- svelte-ignore a11y-missing-attribute -->
    <object class="pdf-embed" data={objectSrc} type="application/pdf" width={width} height={height}>
      <div class="fallback">
        <p>{fallbackText}</p>
        <p>
          <a href={objectSrc} download>Click here to download the PDF.</a>
        </p>
      </div>
    </object>
  {:else}
    <div class="fallback">No PDF provided.</div>
  {/if}
</div>
