<script lang="ts">
  import "../../app.css";
  interface Props {
    rowPosition: number;
    colPosition: number;
    isBlack: boolean;
    isChoose: boolean;
    movePawn: (newRow: number, newCol: number) => void;
  }

  let { rowPosition, colPosition, isBlack, isChoose, movePawn }: Props =
    $props();

  const movePiece = () => {
    movePawn(rowPosition, colPosition);
  };
</script>

<button
  class={`w-full h-full flex items-center justify-center ${
    isChoose ? "border-2 border-yellow-600 bg-yellow-200" : ""
  }`}
  onclick={movePiece}
>
  {#if isBlack}
    <img
      src="https://cdn-icons-png.flaticon.com/128/657/657487.png"
      alt="Black Pawn"
      class="w-[30px] h-[30px]"
    />
  {:else}
    <img
      src="https://cdn-icons-png.flaticon.com/128/657/657588.png"
      alt="White Pawn"
      class="w-[30px] h-[30px]"
    />
  {/if}
</button>
