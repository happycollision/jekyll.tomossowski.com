<script lang="ts">
  let srcIn: string
  export { srcIn as src }
  export let alt: undefined | string = undefined

  function getSources(src: string): string[] {
    if (!src.startsWith("/images")) return [src]
    const original = src
    const fullSize = `/g${src}`

    return [fullSize, original]
  }

  const potentialSources = getSources(srcIn)

  let currentIndex = 0
  $: src = potentialSources[currentIndex]

  function nextSrc() {
    if (potentialSources.length < 2) return
    const nextIndex = (currentIndex + 1) % potentialSources.length

    // prevent an infinite loop of errors
    if (nextIndex > 0) currentIndex = nextIndex
  }
</script>

<img src="{src}" on:error="{nextSrc}" alt="{alt}" {...$$restProps} />
