<script lang="ts">
  import "../../app.css";
  import Pawn from "../../component/pawn/+pawn.svelte";
  import Rook from "../../component/rook/+rook.svelte";

  let pawns = [
    { rowPosition: 1, colPosition: 0, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 1, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 2, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 3, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 4, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 5, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 6, isBlack: false, isChoose: false },
    { rowPosition: 1, colPosition: 7, isBlack: false, isChoose: false },
    { rowPosition: 6, colPosition: 0, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 1, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 2, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 3, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 4, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 5, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 6, isBlack: true, isChoose: false },
    { rowPosition: 6, colPosition: 7, isBlack: true, isChoose: false },
  ];

  let rooks = [
    { rowPosition: 0, colPosition: 0, isBlack: false },
    { rowPosition: 0, colPosition: 7, isBlack: false },
    { rowPosition: 7, colPosition: 0, isBlack: true },
    { rowPosition: 7, colPosition: 7, isBlack: true },
  ];

  const handleClickPawn = (clickedPawn: {
    rowPosition: number;
    colPosition: number;
    isChoose: boolean;
  }) => {
    pawns = pawns.map((pawn) => {
      if (
        pawn.rowPosition === clickedPawn.rowPosition &&
        pawn.colPosition === clickedPawn.colPosition
      ) {
        return {
          ...pawn,
          isChoose: !pawn.isChoose,
          // rowPosition: pawn.isBlack
          //   ? pawn.rowPosition - 1
          //   : pawn.rowPosition + 1,
          // colPosition: pawn.colPosition,
        };
      } else {
        return { ...pawn, isChoose: false };
      }
    });
  };

  const board = Array.from({ length: 8 }, (_, row) =>
    Array.from({ length: 8 }, (_, col) => ({
      row,
      col,
      color: (row + col) % 2 === 0 ? "bg-green-200" : "bg-green-500",
    }))
  );
</script>

<div class="flex flex-col items-center justify-center w-full h-screen">
  <div
    class="grid grid-cols-8 grid-rows-8 w-[560px] h-[560px] border border-black"
  >
    {#each board as row}
      {#each row as cell}
        <div
          class={`flex items-center justify-center w-full h-full ${cell.color}`}
        >
          {#each pawns as pawn}
            {#if pawn.rowPosition === cell.row && pawn.colPosition === cell.col}
              <Pawn
                rowPosition={pawn.rowPosition}
                colPosition={pawn.colPosition}
                isBlack={pawn.isBlack}
                isChoose={pawn.isChoose}
                movePawn={() => handleClickPawn(pawn)}
              />
            {/if}
          {/each}

          {#each rooks as rook}
            {#if rook.rowPosition === cell.row && rook.colPosition === cell.col}
              <Rook
                rowPosition={rook.rowPosition}
                colPosition={rook.colPosition}
                isBlack={rook.isBlack}
              />
            {/if}
          {/each}
        </div>
      {/each}
    {/each}
  </div>
</div>
