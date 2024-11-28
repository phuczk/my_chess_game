<script lang="ts">
  import "../../app.css";
  import Pawn from "../../component/pawn/+pawn.svelte";
  import Rook from "../../component/rook/+rook.svelte";

  let pawns: PawnType[] = [
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

  type PawnType = {
    rowPosition: number;
    colPosition: number;
    isBlack: boolean;
    isChoose: boolean;
  };

  let rooks = [
    { rowPosition: 0, colPosition: 0, isBlack: false },
    { rowPosition: 0, colPosition: 7, isBlack: false },
    { rowPosition: 7, colPosition: 0, isBlack: true },
    { rowPosition: 7, colPosition: 7, isBlack: true },
  ];

  let selectedPawn: { rowPosition: number; colPosition: number } | null = null;

  const handleClickPawn = (clickedPawn: PawnType) => {
    pawns = pawns.map((pawn) => {
      if (
        pawn.rowPosition === clickedPawn.rowPosition &&
        pawn.colPosition === clickedPawn.colPosition
      ) {
        selectedPawn = clickedPawn.isChoose ? null : pawn;
        return {
          ...pawn,
          isChoose: !pawn.isChoose,
        };
      }
      return { ...pawn, isChoose: false };
    });
    const validMoves = getPawnValidMove(clickedPawn);
    console.log(validMoves);
  };

  const getPawnValidMove = (pawn: PawnType) => {
    let validMoves = [];
    const { rowPosition, colPosition, isBlack } = pawn;
    const direction = isBlack ? -1 : 1;
    const startRow = isBlack ? 6 : 1;

    if (
      !pawns.some(
        (p: PawnType) =>
          p.rowPosition == rowPosition + direction &&
          p.colPosition == colPosition,
      )
    ) {
      validMoves.push({ newRow: rowPosition + direction, newCol: colPosition });
    }

    if (
      rowPosition === startRow &&
      !pawns.some(
        (p: PawnType) =>
          p.rowPosition === rowPosition + direction &&
          p.colPosition === colPosition,
      ) &&
      !pawns.some(
        (p: PawnType) =>
          p.rowPosition === rowPosition + direction * 2 &&
          p.colPosition === colPosition,
      )
    ) {
      validMoves.push({
        newRow: rowPosition + direction * 2,
        newCol: colPosition,
      });
    }

    return validMoves;
  };

  const pawnMove = (newPosition: { newRow: number; newCol: number }) => {
    const isOccupied = pawns.some(
      (pawn) =>
        pawn.rowPosition === newPosition.newRow &&
        pawn.colPosition === newPosition.newCol,
    );

    if (isOccupied || !pawns.some((pawn) => pawn.isChoose)) {
      console.log("Vị trí đã bị chiếm hoặc không có quân nào được chọn.");
      return;
    }

    pawns = pawns.map((pawn) => {
      if (pawn.isChoose) {
        return {
          ...pawn,
          rowPosition: newPosition.newRow,
          colPosition: newPosition.newCol,
          isChoose: false,
        };
      }
      return { ...pawn, isChoose: false };
    });

    selectedPawn = null;
  };

  const board = Array.from({ length: 8 }, (_, row) =>
    Array.from({ length: 8 }, (_, col) => ({
      row,
      col,
      color: (row + col) % 2 === 0 ? "bg-green-200" : "bg-green-500",
    })),
  );
</script>

<div class="flex flex-col items-center justify-center w-full h-screen">
  <div
    class="grid grid-cols-8 grid-rows-8 w-[560px] h-[560px] border border-black"
  >
    {#each board as row}
      {#each row as cell}
        <button
          class={`flex items-center justify-center w-full h-full ${cell.color}`}
          onclick={() => pawnMove({ newRow: cell.row, newCol: cell.col })}
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
        </button>
      {/each}
    {/each}
  </div>
</div>
