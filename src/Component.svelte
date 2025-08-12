<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
