<script lang="ts">
  let { rowPosition, colPosition, isBlack } = $props();
  const movePiece = (jump: Number) => {};
</script>

<div class={`w-full h-full flex items-center justify-center`}>
  {#if isBlack}
    <img
      src="https://cdn-icons-png.flaticon.com/128/1626/1626883.png"
      alt=""
      class="w-[30px] h-[30px]"
    />
  {:else}
    <img
      src="https://cdn-icons-png.flaticon.com/128/1626/1626848.png"
      alt=""
      class="w-[30px] h-[30px]"
    />
  {/if}
</div>
