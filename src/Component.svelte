<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  import * as pdfjsLib from 'pdfjs-dist/legacy/build/pdf';
  import workerSrc from 'pdfjs-dist/legacy/build/pdf.worker.entry?worker&inline';

  pdfjsLib.GlobalWorkerOptions.workerPort = new workerSrc();

  export let text

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
