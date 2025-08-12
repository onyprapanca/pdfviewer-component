<script>
  import { getContext } from "svelte"
  import Pdfviewer from "./pdfviewer.svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let text
</script>

<div use:styleable={$component.styles}>
  <Pdfviewer src={text} />
</div>
