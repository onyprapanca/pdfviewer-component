<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  import * as pdfjs from 'pdfjs-dist/build/pdf';

  pdfjs.GlobalWorkerOptions.workerSrc = "https://app.unpkg.com/pdfjs-dist@5.4.54/files/build/pdf.worker.mjs";

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
