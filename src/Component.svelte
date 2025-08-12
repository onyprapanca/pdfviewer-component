<script>
  import { getContext } from "svelte"
  import Pdfviewer from "./pdfviewer.svelte"

  pdfjs.GlobalWorkerOptions.workerSrc = "https://app.unpkg.com/pdfjs-dist@5.4.54/files/build/pdf.worker.mjs";

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <Pdfviewer src={text} />
</div>
