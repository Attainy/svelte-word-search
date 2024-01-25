<script>
    import GridBox from "../components/GridBox.svelte"
    import Header from "../components/Header.svelte";
    export let params;

    const allCreatedGame = JSON.parse(localStorage.getItem("allCreatedGame"));
    const selectSet = allCreatedGame[Number(params.id)]
    const givenWords = selectSet['wordList'];
    const subject = selectSet['subject'];

    // 현재 시간 간단히 표시할 수 있음
    let hour = new Date().getHours();
    let min = new Date().getMinutes();

</script>

<Header />
<div class='game'>
    <div class="game-title">
      <h2>{subject}</h2>
      <div class="info-bar__time">{hour}:{min}</div>
    </div>

    <div class="playbox">
      <GridBox givenWords={allCreatedGame[Number(params.id)]['wordList']}></GridBox>
      <div class="word-list">
        {#each givenWords as val}
            <div>{val}</div>
        {/each}
      </div>
    </div>

</div>
