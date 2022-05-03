<script lang="ts">
  import { onMount } from "svelte";

  const LOCAL_STORAGE_KEY: string = "site-theme";
  const THEME_DEFAULT = "LIGHT";
  let currentTheme = localStorage.getItem(LOCAL_STORAGE_KEY);
  $: color = currentTheme == "DARK" ? "#FFF" : "#000";

  const updateTheme = (newTheme: string) => {
    localStorage.setItem(LOCAL_STORAGE_KEY, newTheme);
    return newTheme;
  };

  onMount(() => {
    if (!currentTheme) updateTheme(THEME_DEFAULT);
  });

  const toggleTheme = () => {
    const newTheme = currentTheme === "DARK" ? "LIGHT" : "DARK";
    currentTheme = updateTheme(newTheme);
  };
</script>

<button on:click={toggleTheme}>
  <svg
    class="w-6 h-6"
    fill={color}
    viewBox="0 0 20 20"
    xmlns="http://www.w3.org/2000/svg"
    ><path
      d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"
    /></svg
  ></button
>
