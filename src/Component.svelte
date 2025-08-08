<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  import * as pdfjsLib from 'pdfjs-dist/legacy/build/pdf';

  // disable worker so it runs inline
  pdfjsLib.GlobalWorkerOptions.workerSrc = null;
  pdfjsLib.disableWorker = true;

  export let text

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
