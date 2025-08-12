<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  import * as pdfjs from 'pdfjs-dist/build/pdf';

  pdfjs.GlobalWorkerOptions.workerSrc = 'pdfjs-dist/build/pdf.worker.min.js';

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
