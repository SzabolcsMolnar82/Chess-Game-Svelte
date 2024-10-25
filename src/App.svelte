<script>
 var t = 'rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1'
 var v
 $: t2 = t.split(" ")[0].split("/").map(v => v.split("").map(q => 
  Number(q) == q ? Array(Number(q)).fill(" ").join("") : q
 ).join(""))
 var fk = {
  "K": "♔",
  "Q": "♕",
  "R": "♖",
  "B": "♗",
  "N": "♘",
  "P": "♙",
  "k": "♔",
  "q": "♕",
  "r": "♖",
  "b": "♗",
  "n": "♘",
  "p": "♙",
  " ": " "
 }
</script>
<main>
  <button 
    type="button" 
    class="btn btn-primary"
    on:click={async () => {
      let ja = 'rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1'
      let answ = await fetch(`https://stockfish.online/api/s/v2.php/?fen=${ja}&depth=10`)
      v = (await answ.json())
      console.log(v);
    }}
  >Primary</button>
    <table>
      {#each t2 as r, i}
      <tr>
        {#each r as c, j}
        <td class="{i % 2 == j % 2 ? 'black' : 'white'}"
          on:drop={e => console.log(
            e.dataTransfer.getData('text'),
            e.dataTransfer.getData('x'),
            e.dataTransfer.getData('y'),
            `${i}`,
            `${j}`
          )}
          on:dragover={e => e.preventDefault()}>
            {#if 'KQRBNP'.includes(c)} 
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <span draggable='true' class='white'
            on:dragstart={e => {
                e.dataTransfer.setData('text', c)
                e.dataTransfer.setData('x', i.toString())
                e.dataTransfer.setData('y', j.toString())
              }}>
            {@html fk[c]}
            </span>
            {:else}
            <span class='black'>
            {@html fk[c]}
            </span>
            {/if}
          </td>
        {/each}
      </tr>
      {/each}
    </table>
</main>

<style>
td {
  border: 1px solid rgb(82, 82, 82);
  width: 40px;
  height: 40px;
  text-align: center;
  text-shadow: 1px 1px 4px black;
  font-size: 25px;
}
td.black {
  background-color: rgb(99, 99, 99);
  color: rgb(156, 156, 156);
}
td.white {
  background-color: rgb(156, 156, 156);
}
table {
  border-collapse: collapse;
  border:solid 4px black;
}
:global(span.white) {
  color: white;
  cursor: grab;
}
:global(span.black) {
  color: black;
  cursor: default;
}
</style>
