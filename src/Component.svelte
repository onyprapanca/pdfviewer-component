<script>
  import { getContext } from "svelte"
  import { onMount } from "svelte";
  let PdfViewer;

  onMount(async () => {
    const module = await import("svelte-pdf");
    PdfViewer = module.default;
  });
    


  export let text

  const { styleable } = getContext("sdk")
  const component = getContext("component")

</script>

<div use:styleable={$component.styles}>
  {#if PdfViewer}
    <svelte:component this={PdfViewer} url={text} />
  {/if}
</div>
