<script>
    import GridBox from "../components/GridBox.svelte"
    import Header from "../components/Header.svelte";
    export let params;

    const allCreatedGame = JSON.parse(localStorage.getItem("allCreatedGame"));
    const selectSet = allCreatedGame[Number(params.id)]
    const givenWords = selectSet['wordList'];
    const subject = selectSet['subject'];

    /* 타이머 */
    let acc = 0;
    function stopWatch() {
      acc = acc + 1;

      let minuteString = (Math.floor(acc / 60)).toString().padStart(2, '0');
      let secondString = (acc % 60).toString().padStart(2, '0');
      
      const timerDiv = document.querySelector('div.info-bar__time');
      timerDiv.innerText = `${minuteString} : ${secondString}`;
    }
    setInterval(stopWatch, 1000);    
</script>

<Header />
<div class='game'>
    <div class="game-title">
      <h2>{subject}</h2>
      <div class="info-bar__time">00 : 00</div>
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
