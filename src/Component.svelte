<script>
  import { getContext } from "svelte"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  const handleClick = () => {
    window.print()
    console.log('handle print')
    component.handleClick()
  }

</script>

<div use:styleable={$component.styles}>
  <button on:click={handleClick}>
    {$component.label}
    <slot />
  </button>
</div>
