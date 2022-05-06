<script>
  import { onMount } from "svelte";

  // imports
  //   import sun from "~/icons/theme/sun.svg";

  const BASE = "/assets/icons";

  export let path;

  let svgRaw = "";
  let classname = "";
  export { classname as class };


  onMount(() => {
    const realPath = `${BASE}/${path}.svg`;
    import(/* @vite-ignore */ realPath)
      .then(({ default: raw }) => {
        svgRaw = raw;
      })
      .catch((err) => console.log(err));
  });
</script>

{#if path}
  <button
    style="min-width: 18px; min-height: 18px"
    class="icon {classname}"
    on:click
  >
    {@html svgRaw}
  </button>
{/if}
