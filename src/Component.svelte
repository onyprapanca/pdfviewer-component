<script>
  import { getContext, onMount, onDestroy } from "svelte";
  import * as pdfjsLib from "pdfjs-dist/legacy/build/pdf";

  export let text; // PDF URL
  const { styleable } = getContext("sdk");
  const component = getContext("component");

  let canvas;
  let ctx;
  let pdfDoc = null;
  let pageNum = 1;
  let totalPages = 0;
  let scale = 1.0;

  // Disable worker for Budibase
  pdfjsLib.GlobalWorkerOptions.workerSrc = null;
  pdfjsLib.disableWorker = true;

  async function loadPdf(url) {
    try {
      const loadingTask = pdfjsLib.getDocument(url);
      pdfDoc = await loadingTask.promise;
      totalPages = pdfDoc.numPages;
      renderPage(pageNum);
    } catch (err) {
      console.error("Error loading PDF:", err);
    }
  }

  async function renderPage(num) {
    const page = await pdfDoc.getPage(num);
    const viewport = page.getViewport({ scale });

    canvas.width = viewport.width;
    canvas.height = viewport.height;

    const renderContext = {
      canvasContext: ctx,
      viewport
    };
    await page.render(renderContext).promise;
  }

  function nextPage() {
    if (pageNum < totalPages) {
      pageNum++;
      renderPage(pageNum);
    }
  }

  function prevPage() {
    if (pageNum > 1) {
      pageNum--;
      renderPage(pageNum);
    }
  }

  function zoomIn() {
    scale += 0.25;
    renderPage(pageNum);
  }

  function zoomOut() {
    if (scale > 0.5) {
      scale -= 0.25;
      renderPage(pageNum);
    }
  }

  onMount(() => {
    ctx = canvas.getContext("2d");
    if (text) {
      loadPdf(text);
    }
  });

  onDestroy(() => {
    if (pdfDoc) pdfDoc.destroy();
  });
</script>

<div use:styleable={$component.styles} style="display: flex; flex-direction: column; gap: 0.5rem;">
  <div style="display: flex; gap: 0.5rem; align-items: center;">
    <button on:click={prevPage} disabled={pageNum <= 1}>Prev</button>
    <span>Page {pageNum} / {totalPages}</span>
    <button on:click={nextPage} disabled={pageNum >= totalPages}>Next</button>
    <button on:click={zoomOut}>-</button>
    <button on:click={zoomIn}>+</button>
  </div>

  <canvas bind:this={canvas} style="max-width: 100%; border: 1px solid #ccc;"></canvas>
</div>
