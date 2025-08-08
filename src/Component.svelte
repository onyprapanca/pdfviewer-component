<script>
  import { getContext } from "svelte"
  import PdfViewer from 'svelte-pdf';
  


  export let text

  const { styleable } = getContext("sdk")
  const component = getContext("component")

</script>

<div use:styleable={$component.styles}>
  <PdfViewer url={text} />
</div>
