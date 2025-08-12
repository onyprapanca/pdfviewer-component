<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  import * as pdfjs from 'pdfjs-dist/build/pdf';

  pdfjs.GlobalWorkerOptions.workerSrc = new URL(
    'pdfjs-dist/build/pdf.worker.mjs',
    import.meta.url
  ).toString();

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
